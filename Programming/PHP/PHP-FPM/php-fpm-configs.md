
# Конфигурация и важные параметры

**Связи:**
- Обратные
	- [PHP-FPM](php-fpm);
- Прямые:
	

**Хештеги:** #Programming/PHP/PHP-FPM

## Глобальная

```ini
[global]
# Основные настройки
pid = /run/php/php8.3-fpm.pid
error_log = /var/log/php8.3-fpm.log
log_level = notice
daemonize = yes
# Аварийный перезапуск при множественных сбоях
emergency_restart_threshold = 10
emergency_restart_interval = 1m
process_control_timeout = 10s
```

## Конфигурация пула

```ini
[www]
# Пользователь и группа
user = www-data
group = www-data
# Сокет для связи с веб-сервером
listen = /run/php/php8.3-fpm.sock
listen.owner = www-data
listen.group = www-data
listen.mode = 0660
# Управление процессами
pm = dynamic
pm.max_children = 50
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 35
pm.max_requests = 1000
# Таймауты и ограничения
request_terminate_timeout = 60s
request_slowlog_timeout = 10s
rlimit_files = 65536
# Мониторинг
pm.status_path = /status
ping.path = /ping
ping.response = pong
# Логирование
slowlog = /var/log/php8.3-fpm-slow.log
catch_workers_output = yes
```

## По типам нагрузки

### Низкая (блоги, landing pages)

```ini
pm = ondemand
pm.max_children = 10
pm.process_idle_timeout = 60s
memory_limit = 128M
```

### Средняя (корпоративные сайты)

```ini
pm = dynamic
pm.max_children = 25
pm.start_servers = 3
pm.min_spare_servers = 2
pm.max_spare_servers = 5
memory_limit = 256M
```

### Высокая (популярные сайты)

```ini
pm = static # или dynamic для очень переменного трафика
pm.max_children = 100
memory_limit = 512M
request_terminate_timeout = 30s
```
