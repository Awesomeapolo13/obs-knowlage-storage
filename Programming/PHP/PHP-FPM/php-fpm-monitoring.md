
# Мониторинг и отладка

**Связи:**
- Обратные
	- [PHP-FPM](php-fpm);
- Прямые:
	

**Хештеги:** #Programming/PHP/PHP-FPM

## Встроенный

Существует страница статуса, которая предоставляет детальную информацию о состоянии PHP-FPM

```bash
# Различные форматы вывода
curl http://localhost/status # Человекочитаемый
curl http://localhost/status?json # JSON для автоматизации
curl http://localhost/status?xml# XML
curl http://localhost/status?full# Детальная информация по процессам
```

Для мониторинга можно выделить несколько ключевых меток:
- `accepted conn` - всего обработано соединений;
- `listen queue` - запросы в очереди (должно быть близко к 0);
- `max listen queue` - максимальная очередь (критично если > 0);
- `active processes` - активные процессы;
- `idle processes` - свободные процессы;
- `max children reached` - сколько раз достигался лимит процессов.

## Анализ производительности

`Slowlog` для выявления узких мест

```ini
slowlog = /var/log/php8.3-fpm-slow.log
request_slowlog_timeout = 5s
request_slowlog_trace_depth = 20
```

Анализ медленных запросов

```bash
# Топ медленных скриптов
awk '$0 ~ /script_filename/ { script = $3 } $0 ~ /\[pool www\]/ && $0 ~ /executing/ { print script }' \
/var/log/php8.3-fpm-slow.log | sort | uniq -c | sort -nr
```

## Системный мониторинг

```bash
# Проверка количества процессов
ps aux | grep php-fpm | wc -l
# Использование памяти всеми процессами PHP-FPM
ps --no-headers -o "rss,cmd" -C php-fpm8.3 | awk '{ sum+=$1 } END { printf "Total RSS: %d MB\n", sum/1024 }'
# Дерево процессов
pstree -p $(pgrep -f php-fpm | head -1)
# Мониторинг в реальном времени
watch -n 1 'curl -s http://localhost/status'
```

## Распространенные проблемы и решения

### Server reached pm.max_children setting

```bash
# Симптом в логах
[WARNING] [pool www] server reached pm.max_children setting (50), consider raising it
```

Решения:
1. Увеличить pm.max_children (если достаточно RAM)
2. Оптимизировать код приложения
3. Добавить RAM или CPU ресурсы
4. Использовать кэширование (Redis, Memcached)

### Высокое потребление памяти

```ini
# Принудительный перезапуск процессов для предотвращения утечек
pm.max_requests = 500

# Мониторинг утечек памяти
php_admin_flag[log_errors] = on
php_admin_value[error_log] = /var/log/php-errors.log
```


## Docker интеграция

```Dockerfile
FROM php:8.3-fpm

# Оптимизированные настройки для контейнеров
RUN docker-php-ext-install opcache pdo_mysql

COPY php-fpm.conf /usr/local/etc/php-fpm.d/www.conf
COPY php.ini /usr/local/etc/php/

EXPOSE 9000
```