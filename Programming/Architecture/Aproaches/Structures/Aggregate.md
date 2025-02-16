
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

2) Методы для управления жизненным циклом, например создание нновой корзины:

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

