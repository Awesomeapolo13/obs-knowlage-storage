# Объект-значение (Value-Object)

**Связи:**
- Обратные:
	- [Паттерны ООП](OOP-patterns);

**Хештеги:** #Programming/OOP/Patterns 

## Определение

**Объект значение (Value Object**) - это специальный тип объектов, который представляет собой неизменяемые данные, не имеющие идентичности и используемые для представления простых значений. Значения объекта являются независимыми от контекста и определяются только через своё состояние. Объекты значения обычно являются иммутабельными, то есть после создания и инициализации их состояние не может быть изменено. Они не поддерживают изменение своего состояния и служат для представления структур данных.

Основная цель объектов значения - обеспечить прозрачность, надежность и устойчивость данных. 

Примерами объектов значения могут быть объекты, представляющие дату, время, деньги, географические координаты и т.д.

## Пример

Пример объекта значения на PHP, представляющего координаты точки (x, y):

```php
class Point
{
    private float $x;
    private float $y;
    
    public function __construct(float $x, float $y) 
    {
        $this->x = $x;
        $this->y = $y;
    }
    
    public function getX(): float
    {
        return $this->x;
    }
    
    public function getY(): float
    {
        return $this->y;
    }
}
```

В этом примере Point - это объект значения, представляющий координаты точки на плоскости. Он имеет два свойства - x и y, представляющие координаты, и методы для доступа к этим значениям (getX() и getY()).

Объект значения Point является неизменяемым. Это делает объект значения более надежным и прозрачным, поскольку гарантируется, что его состояние остается неизменным.

Так же VO может использоваться как часть Entity:

```php

class User
{
    private int $id;
    private string $name;
    private Email $email;

    public function __construct(int $id, string $name, Email $email)
    {
        $this->id = $id;
        $this->name = $name;
        $this->email = $email;
    }

    public function getId(): int
    {
        return $this->id;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function getEmail(): Email
    {
        return $this->email;
    }
}

class Email
{
    private string $address;

    public function __construct(string $address)
    {
        $this->address = $address;
    }

    public function getAddress(): string
    {
        return $this->address;
    }
}

```

В приведенном примере есть два класса: User и Email. User является Entity объектом, представляющим пользователя, а Email - Value Object, представляющий адрес электронной почты.

## Когда применять

1) Когда данные не имеют своей собственной идентичности и используются только для представления простых значений. Например, координаты точек, дата и время, проценты, коды стран и т.д.
2) Когда данные не должны быть изменяемыми.
3)  Когда требуется прозрачность, надежность и устойчивость данных. Значения объекта должны быть независимыми от контекста и определяться только через свое состояние.
4) Когда требуется повысить производительность и эффективность. Объекты значения обычно меньше по размеру и более эффективны в использовании памяти и процессорного времени, чем объекты с более сложной структурой.
5) Когда требуется иметь несколько объектов с одинаковыми значениями. Объекты значения, не имея идентичности, могут быть свободно созданы с одинаковыми значениями и не нуждаются в отдельном управлении уникальности.

## Отличие от DTO

Главное отличие Объекта-значения от Объекта передачи даннных в том, что DTO используется для передачи данных между различными компанентами системы или между клиентом и сервером (т.е. для упаковки и передачи данных). DTO может содержать различные значения от простых типов, до объектов или коллекций объектов. Что же до Value-Object, то он является представлением неизменяемых простых значений, которые не имеют идетичности (координаты, даты, коды и др.). 