
# Безопасность

**Ключи:**
- Обратные:
	- [Redis](redis);
- Прямые:

**Хештеги:** 
- #Programming/Databases/Redis
- #Redis

## После запуска

- работает на порту 6379;
- доступен всем пользователям без аутентификации;
- не шифрует трафик;
- не привязан к конфигурационному файлу.

## Настройка

- создать и подключить конфигурационный файл `/etc/redis/redis.conf`;
- включить protected mode `CONFIG SET protected-mode yes` (есть нюансы при использовании репликации);
- Redis < 6.0: включить аутентификацию по паролю;
```redis
CONFIG SET requirepass <пароль>
AUTH <пароль>
```

- Redis >= 6.0: настройка ACL;
```redis
ACL SETUSER <пользователь> [правила]
AUTH <пользователь> <пароль>
```

- подключить TLS (для безопасного зашифрованного соединения);
```conf
tls-cert-file /path/to/redis.crt
tls-key-file /path/to/redis.key
tls-ca-cert-file /path/to/ca.crt
tls-dh-params-file /path/to/redis.dh
```
