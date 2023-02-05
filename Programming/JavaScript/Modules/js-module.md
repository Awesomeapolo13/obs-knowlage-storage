
# Модули в JS

## Зачем они нужны

- Повторное использование.
- Возможность компоновки.
- Удобство разработки.
- Изоляция.
- Организация проекта.

## История

![[Pasted image 20230122191555.png]]

1) Intermediate Involced Function Expression (IIFE) - первый паттерн для модульности JS. Функции которые можно было выполнить мгновенно один раз.
	- важен порядок выполнения скриптов;
	- работало только в браузерах.
2) Common JS modules - раньше являлся стандартом, на нем был основан  Node.js.
	- использовался в браузере только для сборок bundle;
	- использовался в Node.js для всего.
3) ES modules - стандарт для динамического подключения модулей как в браузере так и в Node.


## Различия

ES module вошел в ноду только в 13 версии.
Поддержка TypeScript есть в обоих способав модулирования.

### CommonJS

- Require в любом месте.
- Можно использовать в условии.
- Загрузка всего из модуля.
- Нет асинхронности.

### ES modules

- Импорт на верхнем уровне.
- Нельзя использовать в условии.
- Выборочная загрузка.
- Асинхронное подключение (тут можно использовать в условии).

## Использование ES modules в Node

- Использовать файлы JS с расширением .mjs (изначальный способ, для обратной совместимости);
- Указать в package.json "type": "module" - тогда все файлы приложения будут интерпертироваться как модули;
- Передать node аргумент --input-type=module

**Ключи:**
- [Программирование](PROGRAMMING);
- [JS](javascript);
- [Node.js](node-js);
- [Comon JS modules](comon-js-module);
- [ES JS modules](es-js-module);

**Хештеги:** #Programming/JS/Module