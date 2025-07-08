# Обработка запросов от веб сервера

**Связи:**
- Обратные
	- [PHP-FPM](php-fpm);
- Прямые:
	

**Хештеги:** #Programming/PHP/PHP-FPM

## Поток обработки запроса

Когда пользователь обращается к PHP-странице, происходит следующая последовательность действий:

1) Браузер отправляет HTTP-запрос
2) Веб-сервер определяет нужно ли обработать этот запрос через php
3) Веб-сервер формирует Fast-CGI-запрос и отправляет его в PHP-FPM
4) Мастер-процесс PHP-FPM получает запрос
5) Мастер назначает запрос свободному воркер-процессу из пуда
6) Воркер выполняет PHP-скрипт
	- инициализирует контекст (RINIT);
	- компилирует код (если нет в OPcache);
	- выполняет скрипт;
	- формирует HTTP-ответ;
	- очищает контекст (RSHUTDOWN)
7) Ответ возвращается через FastCGI отбратно в веб-сервер
8) Веб-сервер отправляет HTTP-ответ браузеру

## Интеграция с OPcache

При включеном OPcache процесс обработки запроса значительно ускоряется.

Без OPCache мы каждый раз:
- читаем PHP-файл с диска;
- выполняем лексический анализ (tokenize);
- парсим AST;
- компилируем в opcodes;
- выполнение opcodes.

С OPcache (после попадания в кеш):
- проверяем есть ли кеш опкодов;
- выполняем готовые опкоды.

## Взаимодействие с веб-серверами

**nginx + PHP-FPM**

```nginx
location ~ \.php$ {
fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
fastcgi_index index.php;
include fastcgi_params;
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
# Оптимизация буферов
fastcgi_buffers 16 16k;
fastcgi_buffer_size 32k;
fastcgi_connect_timeout 60s;
fastcgi_read_timeout 60s;
}
```

**Apache + PHP-FPM**

```apache
<FilesMatch "\.php$">
SetHandler "proxy:unix:/var/run/php/php8.3-fpm.sock|fcgi://localhost/"
</FilesMatch>
# Или через TCP:
ProxyPassMatch "^/(.*\.php(/.*)?)$" "fcgi://localhost:9000/var/www/$1"
```

## Внутренние механизмы: сокеты и FastCGI протокол

**Unix Domain Sockets vs TCP Sockets**

Unix Domain Sockets рекомендуется для локальной связи. Они на 15-20% быстрее TCP-сокетов, но доступны только локально через файловые права.

```ini
listen = /var/run/php/php8.3-fpm.sock
listen.owner = www-data
listen.group = www-data
listen.mode = 0660
```

TCP Sockets лучше использовать для распределенной архитектуры. Они дают возможность вынести PHP-FPM на отдельный сервер, а так же просты для мониторинга сетевых соединений.

```ini
listen = 127.0.0.1:9000
listen.allowed_clients = 127.0.0.1
```

Протокол FastCGI работает через обмен структурированными записями:

```c
ypedef struct {
unsigned char version; // Версия протокола (1)
unsigned char type; // Тип записи
unsigned char requestIdB1; // ID запроса
unsigned char requestIdB0;
unsigned char contentLengthB1; // Длина данных
unsigned char contentLengthB0;
unsigned char paddingLength; // Выравнивание
unsigned char reserved;
} FCGI_Header;
```

Типы записей:

- `FCGI_BEGIN_REQUEST` - начало запроса;
- `FCGI_PARAMS` - переменные окружения (PATH_INFO, QUERY_STRING, etc.);
- `FCGI_STDIN` - входные данные (POST);
- `FCGI_STDOUT` - выходные данные (HTML);
- `FCGI_STDERR` - ошибки;
- `FCGI_END_REQUEST` - завершение запроса.
