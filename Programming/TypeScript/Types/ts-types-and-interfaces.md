
# Types and Interfaces

## Общее

Передача в функцию типизированного объкта в самом общем случае происходит слндующим образом:

```typescript
function compute(coord: {lat: number, long: number}) {  

}
```

Запись выше может быть сокращена при использовании type alias:

```typescript
type coord = {lat: number, long: number};  
  
function compute(coord: coord) {  
  
}
```

Такой способ записи помогает переиспользовать типы.

Ещё один способ - использование интерфейсов:

```typescript
interface ICoord {  
    lat: number;  
    long: number;  
}  
  
function compute(coord: ICoord) {  
  
}
```


Подобным образом можно устанавливать не только объекты. Ниже приведен пример типа, который может содержать либо строку, либо число:

```typescript
type ID = number | string;
```

!! Интерфейсы же могут быть описаны исключительно объектами.


## Наследование интерфейсов и типов

Интерфейсы могут расширяться посредством друг друга (наследоваться):

```typescript
interface Animal {  
    name: string  
}  
  
interface Dog extends Animal {  
    tail?: boolean  
}  
  
const dog: Dog = {  
    name: "sdf",  
}  
  
dog.name;  
dog.tail;
```

То же самое можно проделать и с помощью типов.

```typescript
type Animal = {  
    name: string;  
}  
// Расширяем базовый тип
type Dog = Animal & {  
    tail: boolean;  
}  
  
const dog: Dog = {  
    name: 'Name',  
    tail: true,  
}
```

Использование интерфейсов более предпочтительно и удобно. Например интерфейс может быть расширен по ходу написания кода без объявления ногово интерфейса:

```typescript
// первичное объъявление интерфейса
interface Dog {  
    name: string;  
}  
// Доплнение интерфейса
interface Dog {  
    tail: boolean;  
}  
// Константа имеет оба свойства
const dog: Dog = {  
    name: 'str',  
    tail: false,  
}
```

Т.е. при повторном обхявлении одного и того же интерфейса происходит их слияние. При использовании типов такого функционала не предусмотренно.

**Ключи:**
- [TypeScript](typescript).

**Хештеги:** 
- #Programming/TypeScript/Types
- #Programming/TS/Types