
# Common JS module

1) Создаем файл test.js пишем функцию и указываем что модуль будет экспортировать

```js
const users = ['Word1', 'Word2'];

function greet (name) {
	console.log(`Hello ${name}`);
}

module.exports = {users, greet};
```

2) Берем файл из п.1 и подключаем в другой файл app.js

```js
const {users, greet} = require('./test.js');

for (const user of users) {
	greet(user);
}
```

!! Функция require существует только в node.js, в браузере его нет.

В блоке exports можно делать вернуть что угодно - массив, функцию, объект.
Результаты export переводятся в константу, а значит, если в эту константу что-то будет возвращаться, то это вызовет ошибку.

!! Синтаксис позволяет вызвать модули внутри модулей, что может привести к круговой зависимости между ними. Их нужно избегать.

**Ключи:**
- [Программирование](PROGRAMMING);
- [JS](javascript);
- [Node.js](node-js);
- [Модули JS](js-module);

**Хештеги:** #Programming/JS/Module