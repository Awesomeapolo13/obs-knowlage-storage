
# Exec and spawn

Комманды позволяют выполнить в терминале комманды / скрипты, получить их output и работать с ним далее.

Функционал полезен для написания CLI утилит, для работы проекта.

1) Команда импортируется из модуля child_process.

```js
import { exec } from 'child_process';
```

2) Создаем объект childProcess, которому нужно передать команду и колбек функцию. Сам колбек принимает три аргумента - ошибку, output команды и вывод ошибки:

```js
const childProcess = exec('ls', (err, stdout, stderr) => {  
    if (err) {  
        console.error(err.message);  
    }  
    console.log(`stdout: ${stdout}`);  
    console.log(`stdout: ${stderr}`);  
});
```

3) На этот процесс так же можно подписываться. Например на событие выхода:

```js
childProcess.on('exit', (code) => {  
    console.log(`Код выхода: ${code}`);  
})
```

Результат:

```shell
Код выхода: 0
stdout: example
exercises
index.js
package.json
README.md

stderr: 
```

Команда spawn похожа на exec, но работает по другому (он спавнит указанный процесс). Мы передаем имя процесса в функцию, а затем подписываемся на событие.

```js
import { exec, spawn } from 'child_process';  

const childProcess = spawn('ls')  
  
childProcess.stdout.on('data', (data) => {  
    console.log(`stdout: ${data}`);  
});  
  
childProcess.stderr.on('data', (data) => {  
    console.log(`stderr: ${data}`);  
});  
  
childProcess.on('exit', (code) => {  
    console.log(`Код выхода: ${code}`);  
})
```

Результат будет тем же.

**Ключи:**
- [Node.js](node-js);
- [Event loop](event-loop.md);
- [Worket threads](node-worker-threads.md);

**Хештеги:** 
- #Programming/JS/NodeJs/EventLoop;
- #Programming/JS/NodeJs/Multithreading;