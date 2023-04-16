
# Literal types (Литеральные типы)

Это способ задать строке тип равный конкретной последовательности символов.

```typescript
let b: 'hi' = 'hi';
```

Такой тип может быть полезен, для задания строгого определенного набора строковых значений, например:

```typescript
// Есть два возможных направления, определенные типом  
type direction = 'left' | 'right';  
// Есть функция перемещения  
function moveDog(direction: direction) {
}  
// Верно  
moveDog('left');  
// Тоже верно  
moveDog('right');  
// Ошибка, собака не может двигаться вверх  
moveDog('up');
```

Литералы могут быть и числовыми

```typescript
function moveDog(direction: direction): -1 | 0 | 1 {  
    switch (direction) {  
        case "left":  
            return -1;  
        case "right":  
            return 1;  
        default:  
            return 0;  
    }  
}
```

Возможно комбинирование в использовании литеральных типов.

```typescript
// Интерфейс соединения  
interface IConnection {  
    host: string;  
    port: number;  
}  
// Теперь соединение может быть либо интерфейсом выше, либо дефолтным  
function connect(connection: IConnection | "default") {
}
```

Есть некоторые особенности при работе с литеральным типом. Например:

```typescript
const connection = {  
    host: "localhost",  
    // приравниваем строку к конкретному литералу,   
    // иначе получим ошибку при использовании в функции ниже  
    protocol: "https" as 'https',  
}  
  
function connect(host: string, protocol: 'http' | 'https') {  
}
```

**Ключи:**
- [TypeScript](typescript).

**Хештеги:** 
- #Programming/TypeScript/Types
- #Programming/TS/Types