
# Union types

Это способ задать несколько типов для одной переменной.

```typescript
let universalId: number | string = 5;
```

Запись выше означает, что переменная может быть либо строкой, либо числом.

Если в последующем нам необходимо использовать специфические функции конкретных типов, то необходимо выполнить проверку на этот тип, иначе вы получим ошибку. Например, функция `.toUppercase()` строгового типа данных:

```typescript
function printId(id: number | string) {  
    if (typeof id === 'string') {  
        console.log(id.toUpperCase());  
    }  
    console.log(id);  
}
```


Еще пример, если это строка или массив строк:

```typescript
function helloUser(user: string | string[]) {  
    if (Array.isArray(user)) {  
        console.log(user.join(', ') + ' Hi!');  
    } else {  
        console.log(user + ' Hi!');  
    }  
}
```

**Ключи:**
- [TypeScript](typescript).

**Хештеги:** 
- #Programming/TypeScript/Types
- #Programming/TS/Types