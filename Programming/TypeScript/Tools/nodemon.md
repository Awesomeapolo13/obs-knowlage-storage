# Node monitor

Node monitor - инструмент, который мониторт файлы проекта и при изменениях перезапускает сервер.

Для работы с TS необходим пакет TS node - это своего рода окружение для исполнения TS на Node.js. Он помогает выполнять код TS без предварительной его сборки.

Установка:

```shell
npm i -D nodemon ts-node
```

В корень добавляем файл `nodemon.json`:

```json
{  
  "watch": [  // Директория за которой он будет следить.
    "."  
  ],  
  "ext": "ts,json",  // Расширения за которыми будет следить.
  "ignore": [  
    "src/**/*.spec.ts"  // Игнор файлов (тут с тестами).
  ],  
  "exec": "ts-node --files ./public/index.ts"  // Выполнение скрипта запускающего приложение.
}
```

В файл package.json устанавливаем новый скрипт `dev: nodemon`. Теперь можно запускать проект для разработки командой:

```shell
npm run dev
```

**Ключи:**
- [JavaScript](javascript);
- [TypeScript](typescript);
- [TS Tools](ts-tools);

**Хештеги:** 
- #Programming/TypeScript/Tools
- #Programming/TS/Tools