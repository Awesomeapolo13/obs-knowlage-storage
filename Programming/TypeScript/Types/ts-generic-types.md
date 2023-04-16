
# Generic types

Generic - тип, позволяющий писать универсальные функции, работающие с различными типами данных. Помогает убрать повторение кода и следовать принципу DRY, а так же строить классы.
Дженерики необходимы для устранения повторяющегося кода и описания функций, которые работают с различными типами аргументов и классов.

```typescript
// Передаем дженерик Т  
function log<T>(obj: T): T {  
    console.log(obj);  
    return obj;  
}  
// Теперь можем использовать функцию и со строкой ис числом  
log<string>('asd');  
log<number>(5);
```

Так же можно передавать в функции несколько дженериков.

```typescript
// Передаем сюда и Т и массив  
function log<T, K>(obj: T, arr: K[]): K[] {  
    console.log(obj);  
    return arr;  
}  
  
log<string, number>('asd', [1,2,3]);
```

Мы не можем использовать свойства определенных типов данных, если они не определены прямо (например, свойство length у объектов). Но мы можем сузить наш дженерик.

```typescript
// Создаем интерфейс с нужными нам свойствами  
interface HasLength {  
    length: number;  
}  
// Наследуем дженерик от интерфейса  
function log<T extends  HasLength, K>(obj: T, arr: K[]): K[] {  
    obj.length;  
    console.log(obj);  
    return arr;  
}
```

Так же дженерики могу использоваться для типизации методов интерфейсов.

```typescript
interface IUser {  
    name: string;  
    age?: number;  
    bid: <T>(sum: T) => boolean  
}

function bid<T>(sum: T): boolean {  
    return true;  
}
```

**Ключи:**
- [TypeScript](typescript).

**Хештеги:** 
- #Programming/TypeScript/Types
- #Programming/TS/Types