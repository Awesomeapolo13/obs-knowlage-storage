
# Оценка производительности

## Модуль `perfomance`

Для оценки производительности существует встроенная в Node.js библиотека `perfomance`. Она позволяет с помощью меток измерять производительность участков кода.

```js
function slow() {  
    // отметка по времени для начала оценки  
    performance.mark('start');  
    const arr = [];  
    for (let i = 0; i < 10000000; i++) {  
        arr.push(i*i);  
    }  
    // Отметка времени для окончания оценки  
    performance.mark('end');  
    // Определяем имя для результата измерений и сам результат.  
    performance.measure('slow', 'start', 'end');  
    // Получаем результат измерений по его имени.  
    console.log(performance.getEntriesByName('slow'));  
}  
  
slow();
```

В итоге вывод:

```shell
[
  PerformanceMeasure {
    name: 'slow', // имя измерения
    entryType: 'measure', // тип
    startTime: 37.79994500055909, // когда начали
    duration: 140.79722500033677, // когда закончили
    detail: null
  }
]
```

Однако такие функции как `console.log()` не слишком удобны, особенно для оценки произволительности на проде (т.к. их надо расставить, а затем удалить и т.д.).

## Observer хуки

Хуки - это возможность на что-то подписаться, например, на измерение чего либо.
Для их использоватения необходимо создать обозреватель (observer) и передать ему entry для имерения. При этом функция выше остается без изменений.

```js
const perf_hooks = require("perf_hooks");  
  
// Создаем наблюдатель  
const performanceObserver = new perf_hooks  
    // На вход принимает items - это метки, измерения и прочее, и наблюдателя.  
    .PerformanceObserver((items, observer) => {  
        // Выведет все entries  
        console.log(items.getEntries());  
        // Т.к. возвращает массив берем последний элемент.  
        const entry = items.getEntriesByName('slow').pop();  
        // Выведет конкретную entry.  
        console.log(`${entry.name}: ${entry.duration}`);  
        // отключаем наблюдатель.  
        observer.disconnect();  
    })  
// Сообщаем наблюдателю, чтобы он обозревал entry types, переданные в ключах объекта.  
// Это те виды entry, за которыми он должен следить и триггерить колбек (хук).  
  
performanceObserver.observe(  
    {
	    entryTypes: ['measure']  
    });
```

Вывод:

```shell
[
  PerformanceMeasure {
    name: 'slow',
    entryType: 'measure',
    startTime: 24.935244999825954,
    duration: 123.92132500000298,
    detail: null
  }
]
slow: 123.92132500000298
```

Теперь можно измерять производительность кода не изменяя его, а лишь передавая фича флаг включать вывод измерений.
Минус в том, что весь код надо размечать.

**Ключи:**
- [Node.js](node-js);

**Хештеги:** #Programming/JS/NodeJs