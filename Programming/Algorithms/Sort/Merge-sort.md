
# Алгоритм сортировки слиянием #

## Зачем нужен ##

Сортировка списка
Основан на делении массива попалам и заполнении нового массива по результатам сравнения элементов из двух этих половин.

Временная сложность -  $O(n*log(n))$.
Работает стабильнее чем быстрая сортировка.

## Условия для использования ##
Нет

## Алгоритм ##

1) Делим исходный массив попалам (или почти, в зависимости от количества элементов);
2) Сортируем каждую из половин массива;
3) Если половины слишком большие рекурсивно проводим операци. п.1.;
4) Сливаем отсортированные половины массива:
	 * сравниваем первые элементы двух паловин;
	 * меньший элемент уклыдываем в новый массив;
	 * оставшийся от сравнения на предыдщем шаге элемент сравниваем сл следующем в другой половине;
	 * и т.д.

## Реализация ##

Реализацяи на JS:

```javascript
/**  
 * Сортирует половины массивов слиянием 
 * @param leftArr  
 * @param rightArr  
 * @returns {*[]}  
 */  
function merge(leftArr, rightArr) {  
    let resultArr = [];  
    let i = 0;  
    let j = 0;  
  
    while (i < leftArr.length && j < rightArr.length) {  
        resultArr.push(  
            (leftArr[i] < rightArr[j]) ? leftArr[i++] : rightArr[j++]  
        );  
    }  
  
    return [  
        ...resultArr,  
        ...leftArr.slice(i),  
        ...rightArr.slice(j)  
    ];  
}  
  
/**  
 * Сортировка слиянием 
 * @param sortedArr  
 * @returns {*[]}  
 */  
function mergeSort(sortedArr) {  
    if (  
        !sortedArr && !sortedArr.length  
    ) {  
        throw new Error('Не верный формат аргументов в функции mergeSort!');  
    }  
  
    let arrLength = sortedArr.length;  
  
    if (arrLength === 1) {  
        return sortedArr;  
    }  
  
    let middle = Math.round(arrLength / 2);  
    // делим массив на две части  
    let leftArr = sortedArr.slice(0, middle)  
    let rightArr = sortedArr.slice(middle);  
  
    // Повторяем разделение массивов в обеих частях новых массивов.  
    // Вызываем функцию для слияния массивов    
    return merge(  
        mergeSort(leftArr),  
        mergeSort(rightArr)  
    );  
}
```

**Хештеги**:
* #Programming/Algorithms/Sort;
* #Programming/Algorithms/Recursion 