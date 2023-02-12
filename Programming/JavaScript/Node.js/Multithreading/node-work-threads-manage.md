
# Создание и управление worker threads

1) Создать для форкера отдельный модуль.
2) Переместить туда функции требующие объемной работы.

```js
import {factorial} from "../factorial.js";  
  
const compute = ({array}) => {  
    const  arr = [];  
    for (let i = 0; i < 100000000; i++) {  
        arr.push(i+i);  
    }  
  
    return array.map(e1 => factorial(e1));  
}
```

3) Добавляем способ получения данных воркером через встроенный модуль `worket_threads`. `ParentPort` - виртуальный порт, куда мы отправляем ответы воркера, `workerData` - данные для обработки.

```js
import { parentPort, workerData} from 'worker_threads';
```

4) Читаем данные из `workerData`, выполняем нашу операцию, возвращаем ответ в порт.

```js
// Отправляем сообщение с результатом работы функции  
parentPort.postMessage(compute(workerData));
```

5) Вызываем воркер из файла где он нужен.

```js
import { Worker } from 'worker_threads';  
  
const compute = (array) => {  
    // Вызываем воркеры через промисы  
    return new Promise((resolve, reject) => {  
        // Передаем файл, который нужно запустить в отдельном потоке,  
        const worker = new Worker(  
            './workers/worker.js',  
            // Передаем данные для работы воркера.  
            {  
                workerData: {  
                    array  
                }  
            }  
        );  
    });  
}
```

6) Далее необходимо прибегнуть к использованию событий, для обработки результатов работы воркера.

```js
// Обрабатываем результат работы (сообщение).  
worker.on('message', (msg) => {  
    // Идентификатор потока.  
    console.log(worker.threadId);  
    resolve(msg);  
});  
// Обрабатываем ошибку.  
worker.on('error', (err) => {  
    reject(err);  
});  
// Обработка события завершения работы.  
worker.on('exit', () => {  
    // Если попытаться тут посмотреть threadId, то он будет = -1, т.к. воркер завершился  
    console.log('Завершил работу');  
});
```

7) Теперь место где используется функция с воркером, нам необходимо настроить на ожидание всех промисов.

```js
const main = async () => {  
    performance.mark('start');  
    // Используем all, чтобы дождаться выполнения всех наших событий  
    const res = await Promise.all(  
        [            compute([25, 20, 19, 48, 30, 50]),  
            compute([25, 20, 19, 48, 30, 50]),  
            compute([25, 20, 19, 48, 30, 50]),  
            compute([25, 20, 19, 48, 30, 50]),  
        ]  
    );  
  
    console.log(res);  
  
    performance.mark('end');  
    performance.measure('main', 'start', 'end');  
    console.log(performance.getEntriesByName('main').pop());  
}
```

!! Если делаете web-серверы, то создание воркера на каждый запрос является угрозой безопасности (DDos атаки). Необходимо выделить набор воркеров, которые будут работать в фоне, и отдавать им задачи, если они доступны. Т.е. необходимо ограничить поток воркеров.

**Ключи:**
- [Node.js](node-js);
- [Event loop](event-loop.md);
- [Timer](node-timer);
- [Worket threads](node-worker-threads.md);

**Хештеги:** #Programming/JS/NodeJs/EventLoop