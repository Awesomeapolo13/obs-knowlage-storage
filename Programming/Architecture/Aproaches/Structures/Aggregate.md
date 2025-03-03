
# Аггрегат (Aggregate)

**Ключи:**
- Обратные:
	- [DDD](DDD);
	- [Тактическое проектирование](DDD-tactical-design);
	- [Список структурных элементов](Structural-element);

**Хештеги:** #Programming/Arch/Structures

## Общая концепция

**Аггрегат** - концептуальное понятие, включающее корневую сущность, и другие принадлежащие ей сущности, объекты значения. Это своего рода класстер связанных объектов, которые рассматриваются как единое целое с точки зрения изменения данных.

Такая композиция способна обеспечить согласованность транзакций в пределах границы агрегата. В случае изменения состояния агрегата, должны быть обеспечены бизнес-инварианты.

То есть при сохранении аггрегата должны сохраняться, обновляться или удаляться все связанные с ним сущности без каких либо задержек.

У аггрегата всегда есть корневая сущность (aggregate root), через которую происходит взаимодействие с внешним миром.

## Важные характеристики

1) Транзакционная согласованность: все изменения внутри агрегата должны быть атомарными. Если вы обновляете корзину, все её элементы должны обновиться в рамках одной транзакции.
2) Ссылочная целостность: объекты за пределами агрегата могут ссылаться только на корень агрегата, но не на его внутренние сущности. Например, Order может ссылаться на Basket, но не должен напрямую ссылаться на BasketItem.
3) Инкапсуляция бизнес-правил: все правила, касающиеся взаимодействия между объектами внутри агрегата, должны быть реализованы внутри него.

## Обычные аттрибуты агрегатов

1) Идентификатор, желательно в виде value object.

```php
class Basket
{
    private BasketId $id;  // Value Object для идентификатора
    
    public function getId(): BasketId 
    {
        return $this->id;
    }
}
```

2) Методы для управления жизненным циклом, например создание новой корзины:

```php
class Basket
{
    public static function create(UserId $userId): self 
    {
        $basket = new self(BasketId::generate(), $userId);
        // Публикация доменного события
        $basket->recordEvent(new BasketCreated($basket->getId()));
        return $basket;
    }
}
```

3) Методы для проверки состояния:

```php
class Basket
{
    public function isReadyForOrder(): bool 
    {
        return !$this->isEmpty() 
            && $this->allItemsAvailable() 
            && $this->hasValidDelivery();
    }
    
    private function allItemsAvailable(): bool 
    {
        return $this->items->forAll(fn(BasketItem $item) => $item->isAvailable());
    }
}
```

4) Коллекция доменных событий, которые накапливаются по мере выполнения юскейса и реализуются после сохранения корзины:

```php
class Basket
{
    private Collection $domainEvents;
    
    protected function recordEvent(DomainEvent $event): void 
    {
        $this->domainEvents->add($event);
    }
    
    public function releaseEvents(): array 
    {
        $events = $this->domainEvents->toArray();
        $this->domainEvents->clear();
        return $events;
    }
}
```

## Размер аггрегатов

Поскольку аггрегат хранит множество бизнес правил и связанных сущностей, он может выйти очень раздутым. Существует принцип "Держите аггрегаты маленькими".

Наличие аггрегата большого размера породит проблемы:
- конкурентность - два пользователя пытаются изменить большой аггрегат одновременно, но он блокирует большое количество связанных записей;
- производительность - даже при внесении небольших изменений приходится вытаскивать аггрегат целиком;
- согласованность - трудно поддерживать в согласованном состоянии большое количество связанных объектов.

Например корзина связана с заказом и пользователем. Но это не значит что нам нужен здоровый аггрегат для получения всей этой связки. Лучше разделить их на отдельные аггрегаты, связанные по id.

```php
class Order
{
    private OrderId $id;
    private CustomerId $customerId;  // Ссылка на другой агрегат через ID
    private BasketId $basketId;      // Ссылка на другой агрегат через ID
    private OrderStatus $status;
    private Collection $orderItems;
    
    // Обратите внимание: мы не храним полную информацию о клиенте
    // или корзине, только их идентификаторы
}
```

## Инварианты

Инварианты  - бизнес правила, которые должны оставаться верными всегда, на протяжении всего жизненного цикла аггрегата. Они всегда должны быть частью аггрегата, т.к. это граница согласованности данных. Но на протяжении процесса разработки, правила могут усложняться и раздувать аггрегат кодом.

Для улучшения организации инвариантов существует несколько способов:

- хранение инвариантов в объектах-значениях;
- выделение их в доменные сервисы;
- выделение их в спецификации.

### Инварианты в объектах-значениях

Если инвариант не требует внешнего взаимодействия (получения данных из стороннего API), и относится к какому то одному значению, то лучше использовать value-object. Например, ограничения по количеству:

```php
class BasketItemQuantity
{
    private function __construct(
        private readonly int $value
    ) {}

    public static function create(int $quantity, int $minimumAllowed): self
    {
        if ($quantity < $minimumAllowed) {
            throw new InvalidQuantityException(
                "Quantity {$quantity} is less than minimum allowed {$minimumAllowed}"
            );
        }

        return new self($quantity);
    }
}
```


Теперь при создании объекта значения будет срабатывать инвариант.

### Инварианты в доменных сервисах

Если инвариант требует внешних взаимодействий (получение данных со стороны других API) то лучше использовать доменный сервис.

```php
class BasketProductValidationService
{
    public function __construct(
        private ProductApiClient $productApi,
        private StockService $stockService
    ) {}

    public function validateProductForBasket(ProductId $productId, int $quantity): Result
    {
        // Получаем актуальные данные о продукте
        $productData = $this->productApi->getProduct($productId);
        if ($productData === null) {
            return Result::failure('Product not found');
        }

        // Проверяем остатки
        $stockAvailable = $this->stockService->checkAvailability($productId, $quantity);
        if (!$stockAvailable) {
            return Result::failure('Insufficient stock');
        }

        return Result::success($productData);
    }
}
```

Причины использования:

- сервис инкапсулирует всю работу с внешними системами;
- упрощается тестирование - можно легко подменить внешние зависимости;
- агрегат остается чистым и сфокусированным на своей основной логике

При этом использовать доменный сервис нужно так же в пределах аггрегата. Например, передавая его в качестве аргемента:

```php
class Basket
{
    public function addProduct(
        ProductId $productId, 
        int $quantity,
        BasketProductValidationService $validationService
    ): void {
        // Проверяем внешние правила
        $validationResult = $validationService->validateProductForBasket(
            $productId, 
            $quantity
        );
        
        if (!$validationResult->isSuccess()) {
            throw new BasketOperationException($validationResult->getError());
        }

        $productData = $validationResult->getData();
        
        // Создаем Value Object, который проверит внутренние правила
        $itemQuantity = BasketItemQuantity::create($quantity, $productData->getMinimumQuantity());
        
        // Проверяем правила, связанные с состоянием корзины
        if ($this->hasDelivery() && $productData->containsAlcohol()) {
            throw new BasketOperationException('Cannot deliver alcohol');
        }

        $this->items->add(new BasketItem($productId, $itemQuantity, $productData->getPrice()));
        $this->recordEvent(new ProductAddedToBasket($this->id, $productId, $quantity));
    }
}
```

При этом все инварианты по прежнему проверяются внутри аггрегата.

### Инварианты в спецификациях



Добавить

- про ссылки аггрегтов друг на друга
- про инварианты и способы их огранизации