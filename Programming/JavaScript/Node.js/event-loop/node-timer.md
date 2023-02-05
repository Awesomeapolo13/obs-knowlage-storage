# Таймеры

## Создание таймера

Таймеры это функции выполняемые с задержкой времени.

пример:

```js
const start = performance.now();  
setTimeout(() => {  
    console.log(performance.now() - start);  
    console.log('Прошла секунда');  
}, 1000);
```

При этом время выполнения этой функции будет не 1 секунда, а примерно 1001.99408899 и т.д. Это объясняется тем, что мы тратим некоторое время на то, чтобы дойти до фазы таймеров в event-loop node.js.

Так же таймер можно вызвать передав функцию аргументом:

```js
function myFunc(arg) {  
    console.log(`Аргумент => ${arg}`)  
}  
  
setTimeout(myFunc, 1500, 'Первый');
```

Существует функция `setImmediate()`, которая выполняется после всего (после полного очищения стека). Функция `setImmediate()` выполняется в фазу check, поэтому выполнится позже `setTimeout()` при передаче времени задержки 0ms (и если не используем es-modules).

```js
console.log('Before');  
// Мгновенное выполнение функции, но после всего (после того как стек очистится совсем).  
setImmediate(() => {  
    console.log('After all!')  
});  
  
console.log('After');
```

Результат выполнения:

```shell
Before
After
After all!
```

## Отмена таймера

Отмена таймера происходит происходит фунцией `clearTimeout()`. При этом в аргументы функции необходимо передать `id`, отменяемого таймера.  Этот `id` возвращается при вызове `setTimeout()`.

```js
// Пишем id таймера в константу.
const timerId = setTimeout(() => {  
    console.log('BOOM');  
}, 5000);  
// Отменяем его.
setTimeout(() => {  
    clearTimeout(timerId)  
}, 1000);
```

Аналогичные действия применяются и для функции `setInterval()`.

**Ключи:**
- [Node.js](node-js);
- [Event loop](event-loop.md);

**Хештеги:** #Programming/JS/NodeJs/EventLoop