
# Различия сред выполнения JS

## Браузер

1) Интерактивные web-приложения (React.js, View.js, есть дерево компаонентов страницы).
2) Есть API, касающийся браузера (Browser API или DOM, window и др.).
3) Фрагментация версии - то есть запуская код в браузере мы не знаем какой версии у нас будет движок. Делая приложения нужно учитывать совместимость со старыми версиями языка. 
4) ES modules.

## Node js

1) Серверные приложения.
2) Есть Node API (файловая система, сеть, модули криптографии и др.). Оно не совместимо с браузером.
3) Единая версия, которая находится на сервере, не нужно переживать за обратную совместимость.
4) ES modules + Common js.
5) Может выполнять только код на JS.
6) NPM 
7) Нет ограничений безопасности (не задумывался как секьюрная вещь, при запуске имеет доступ ко всей файловой системе).

## Deno

Среда выполнения JS кода на сервере. Создавалась теми же, кем и Node.js.

1) Можно выполнять код на JS, TS (под капотом все равно компилирует TS в JS).
2) Вместо NPM подключаются модули .ts .js, пути к которым пишем руками.
3) Контроль безопасности CLI
4) API совместимое с браузером.
5) Небольшое комьюнити и непредсказуемые баги на проде.


**Ключи:**
- [Программирование](PROGRAMMING);
- [JS](javascript);
- [Node.js](node-js);

**Хештеги:** #Programming/JS