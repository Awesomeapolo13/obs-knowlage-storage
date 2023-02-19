
# Fork

Еще один способ выделять процесс в отдельный поток. В отличие от spawn и exec (где можно запускать любой процесс) fork запускает процесс с указанным файлом.

```js
import { fork } from 'child_process';

const forkProcess = fork('fork.js');  
// Подписываемся на изменение (тут на событие ответа нашего процесса)  
forkProcess.on('message', (msg) => {  
    console.log(`Получено сообщение: ${msg}`);  
});  
  
forkProcess.on('close', (code) => {  
    console.log(`Exited code: ${code}`);  
});
```

Чтобы отправить сообщение в процесс:

```js
// Чтобы передать сообщение нашему процессу (принимает сообщение и колбек, если такой есть с ошибкой)  
forkProcess.send('Ping');
```

Теперь посмотрим как получать сообщения из процесса.
В отдельном файле обратимся к глобальной переменной `process`, которая имеет внутри различные подписки, информацию о платформах и других процессах. В нем мы с помощью колбека обработаем сообщение и отправим ответ нашему дочернему процессу.

```js
process.on('message', (msg) => {  
    console.log(`Клиент получил: ${msg}`);  
    process.send('Pong!');
    // Для завершения процесса отсоединяем процесс  
	process.disconnect();
	// Чтобы не получить исключения, используем retutn
	return;
});
```

**Ключи:**
- [Node.js](node-js);
- [Event loop](event-loop.md);
- [Worket threads](node-worker-threads.md);
- [Exec and spawn commands](exec-and-spawn-command);

**Хештеги:** 
- #Programming/JS/NodeJs/EventLoop;
- #Programming/JS/NodeJs/Multithreading;