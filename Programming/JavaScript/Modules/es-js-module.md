
# ES module

1) Создадим файл test2.mjs и сипользуем в нем ключевое слово *export*.

```js
export const users = ['Word1', 'Word2'];

export function greet(name) {
	console.log(`Привет ${name}`)
}
```

2) В файле app.mjs вызовем модуль из п.1 с помощью ключевого слова import.

```js
import { users, greet } from './test2.mjs';

for (const user of users) {
	greet(user);
}
```

3) Можно импортировать вообще все одной командой.

```js
import * as char from './test2.mjs';
```

4) Так же существует дефолтный экспорт.

```js
export default function log() {
	console.log('Everithing');
}
```

Теперь импортируем дефолтную функцию:

```js
import log from  './test2.mjs';
```

5) Если нам нужен не только дефолтный импорт но и остальные, то импортируем остальные части после дефолтного. Так же каждая из функций может быть переопределена на этопе импорта.

```js
import log, { users, greet as hello } from './test2.mjs';
// or
import log, * as char from './test2.mjs';
```

6) Существует глобальная функция импорта. Эта функция возвращает Promise, поэтому необходимо использовать await, для получения функций.

```js
async function main() {
	const { characters, greet } = await import('./test2.mjs');
}

main();
```

Такой импорт модулей называется асинхронным и позволяет использовать их в любой части кода.

**Ключи:**
- [Программирование](PROGRAMMING);
- [JS](javascript);
- [Node.js](node-js);
- [Comon JS modules](comon-js-module);
- [Модули JS](js-module);
- [ES JS modules](es-js-module);

**Хештеги:** #Programming/JS/Module