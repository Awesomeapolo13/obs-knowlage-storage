
# Basic types

К базовым типам относятся числа, строки и булево значение (`number`, `string`, `boolean`).

Задачть переменную опредленного типа:

```typescript
let a: number = 5;
let b: string = 'sdfsc';
```


Теперь если в первую переменную попробовать записать строчное значение, то мы получим ошибку.

При этом автоматическое приведение типов все еще будет работать. За исключением случаев, когда мы указываем результирующий тип переменной.

```typescript
let c = a + b; // в итоге будет строка, т.к. это конкатенация
let c: number = a + b; // будет ошибка
let c: number = a + Number(b); // явное приведение к типу для устранения проблемы
```

Похожим образом можно задать массив. Как правило это списки элементов одинаковых типов (строк, чисел). Если нужно задать массив с элементами разных типов, то нужно использовать tuples (кортежи).

```typescript
let names: string[] = ['sd', 'vdfcdfc']; // тут должен быть список только строк
let tup: [number, string] = [1, 'dfgdfg']; // тут допустимо одно число и строка
```

При этом в кортежах нужно прописывать тип каждого элемента списка. Если хотя бы у одного элемента не будет типа, то будет ошибка.

!! В кортежи, из-за обратной совместимости, можно пушить новые элементы, но обратиться к ним по индексу не получится.

Для совсем любых массивов и переменных существует тип `any`. Его не рекоммендуется использовать, плюс в tsconfig.json может стоять запрет для использования этого типа.

```typescript
let e: any = 3;  
e = 'sfvs';  
e = true;
```


Для правильной типизации функций нужно типизировать аргументы ввода и вывода.

```typescript
function greet(name: string): string {  
    return name + ' Hi';  
}

names.map((x: string): string => x);
```

Для работы с объектами используются объектные типы, однако в чистом виде они используются довольно редко. Вместо них используются Interfices и Types. В целом при обявлении объектного типа, надо указать тип каждому свойству, при этом необязательные свойства могут быть отмечены символом `?`. Этот символ отмечает, что свойство может быть либо указанного типа либо `undefined`.

```typescript
function coord(coord: {lat: number, long?: number}) {  
  
}
```



**Ключи:**
- [TypeScript](typescript).

**Хештеги:** 
- #Programming/TypeScript/Types
- #Programming/TS/Types