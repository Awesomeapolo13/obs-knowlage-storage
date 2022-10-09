
# Алгоритм быстрой сортировки #

## Зачем нужен ##

Для быстрой сортировки списка.
Алгоритм основан на свойстве отсортированного масива. 
Массив отсортирован тогда и только тогда, когда для любого его элемента все элементы, находящиеся слева, не больше этого элемента, в все, находящтеся справа - не меньше.

Временная сложность -  $O(n*log(n))$.
Может достигать квадратичного времени на плохо отсортированных массивах.

## Условия для использования ##
Нет

## Алгоритм ##

1) Берем первый элемент массива (нулевой);
2) Все элементы массива, что меньше его, помещаем левее;
3) Повторяем эту процедуру для обеих половин массива (они поделены элементом из п.1).

## Реализация ##

Реализацяи на JS:

```javascript

/**  
 * Функция сортировки массива по частям, для использования в рамках быстрой сортировки * @param sortedArr  
 * @param from  
 * @param to  
 * @returns {(*)[]}  
 */  
function partition(sortedArr, from, to) {  
    const first = sortedArr[from];  
    for (let i = from; i < to; i++) {  
        if (first > sortedArr[i]) {  
            let storage = sortedArr[i];  
            sortedArr[i] = first;  
            sortedArr[i - 1] = storage;  
        }  
    }  
  
    return [  
        sortedArr.indexOf(first), // Новый индекс для сортировки  
        sortedArr  
    ];  
}
  
/**  
 * Реализация быстрой сортировки * @param sortedArr - сортируемый массив  
 * @param from  
 * @param to  
 * @returns {*}  
 */  
function quickSort(  
    sortedArr,  
    from = 0,  
    to = sortedArr.length  
) {  
    // проверка типов входных данных  
    if (  
        typeof (sortedArr) !== "object" ||  
        typeof (from) !== "number" ||  
        typeof (to) !== "number"  
    ) {  
        throw new Error('Не верный формат аргументов в функции binarySearch!');  
    }  
  
    let pivot = 0;  
  
    if (from < to) {  
        // метод, который выбирает элемент и все элементы, что меньше него перекидывает влево  
        // и возвращает новый индекс центрального элемента        
        [pivot, sortedArr] = partition(sortedArr, from, to);  
        // вызываем метод quickSort для правой и левой частей массива  
        quickSort(sortedArr, from, pivot - 1);  
        quickSort(sortedArr, pivot + 1, to);  
    }  
  
    return sortedArr;  
}
```

**Хештеги**:
* #Programming/Algorithms/Sort;
* #Programming/Algorithms/Recursion 