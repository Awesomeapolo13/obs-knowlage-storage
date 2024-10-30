
# PHP-FPM (FastCGI Process Manager)

**Связи:**
- Обратные
	- [PHP](PHP);
- Прямые:
	

**Хештеги:** #Programming/PHP

## Определение

**PHP-FPM (FastCGI Process Manager)** — это специальная реализация FastCGI для PHP, которая значительно улучшает производительность PHP приложений, особенно на высоконагруженных серверах.

## Зачем нужен PHP-FPM:

1. **Обработка множества запросов**: PHP-FPM позволяет обрабатывать множество параллельных запросов, управляя пулом PHP-процессов. Это позволяет избежать затрат на создание и завершение процессов при каждом запросе.
    
2. **Повышение производительности**: Он обеспечивает более эффективное управление ресурсами по сравнению с традиционным модулем PHP, работающим в составе веб-сервера (например, Apache).
    
3. **Настраиваемые пулы процессов**: PHP-FPM позволяет настраивать разные пулы процессов для разных доменов или приложений, задавая индивидуальные параметры (количество процессов, лимиты ресурсов и т.д.).
    
4. **Мониторинг и логирование**: В PHP-FPM встроены инструменты мониторинга и логирования, что упрощает управление и отладку.
    
5. **Демонизация**: PHP-FPM работает как демонический процесс, поддерживающий постоянные соединения, что снижает нагрузку на сервер.

## Взаимодействие с NGINX

Когда Nginx получает запрос на обработку PHP-скрипта, он передает этот запрос PHP-FPM через протокол FastCGI. PHP-FPM выполняет PHP-код и возвращает результат обратно в Nginx, который затем отправляет его пользователю.

### Основные этапы

- **Получение запроса Nginx**:
    
    - Клиент (например, браузер) отправляет HTTP-запрос на сервер Nginx.
    - Если запрос адресован PHP-скрипту, Nginx определяет это на основе конфигурации и перенаправляет запрос на PHP-FPM.

- **Передача запроса в PHP-FPM**:
    
    - Nginx использует модуль `fastcgi_pass`, чтобы передать запрос на обработку PHP-FPM.
    - Nginx может передавать запрос через UNIX-сокет (например, `/run/php/php7.4-fpm.sock`) или через TCP-соединение (например, `127.0.0.1:9000`).

- **Обработка запроса PHP-FPM**:
    
    - PHP-FPM получает запрос и выбирает один из процессов из пула для его обработки.
    - PHP-интерпретатор выполняет PHP-код, взаимодействует с базой данных или другими сервисами, если необходимо, и формирует ответ.

- **Возврат результата в Nginx**:
    
    - Результат выполнения PHP-скрипта (обычно это HTML-код) передается обратно в Nginx.
    - Nginx отправляет этот результат обратно клиенту.

### Конфигурация и ключевые моменты

```nginx
server {
    listen 80;
    server_name example.com;
    root /var/www/html;

    # Основной обработчик для всех запросов
    location / {
        try_files $uri $uri/ =404;
    }

    # Обработка PHP-скриптов
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;

        # Путь к PHP-FPM (сокет или TCP)
        fastcgi_pass unix:/run/php/php7.4-fpm.sock;
        # fastcgi_pass 127.0.0.1:9000;

        # Передача дополнительных параметров (если нужно)
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    # Обработка статических файлов
    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires max;
        log_not_found off;
    }
}
```

1) **`location ~ \.php$`**: Этот блок определяет, что все файлы с расширением `.php` должны обрабатываться PHP-FPM.
    
2) **`fastcgi_pass`**: Здесь указывается адрес, по которому Nginx передает запросы на обработку PHP-FPM. Это может быть UNIX-сокет или TCP-порт.
    
3) **`SCRIPT_FILENAME`**: Эта директива передает путь к PHP-скрипту в PHP-FPM, чтобы тот знал, какой файл нужно выполнить.

### Преимущества

- **Производительность**: Nginx известен своей высокой производительностью и низким потреблением памяти, что делает его идеальным выбором для работы с PHP-FPM на высоконагруженных сайтах.
    
- **Гибкость конфигурации**: Nginx позволяет легко настраивать различные параметры для обработки запросов, таких как кеширование, перенаправления, ограничения по доступу и многое другое.
    
- **Безопасность**: Разделение веб-сервера и обработчика PHP-запросов (Nginx и PHP-FPM) повышает безопасность, так как можно жестко ограничить доступ к PHP, оставляя Nginx на передовой линии для обработки всех внешних запросов.
