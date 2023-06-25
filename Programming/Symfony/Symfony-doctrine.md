
# Установка и настройка доктрины

1) Вводим команду по установке ОРМ пакета. На вопрос об установке дополнительных конфигурация для докера отвечаем - x, т.к. будем сами конфигурировать.

```shell
composer require symfony/orm-pack
```

2) Если нужно перенести  миграции для правильности архитектуры, то стоит перенести ее в папку с ядром Shared/Infrastructure/Database/Migrations
3) Переопределяем путь к директории для миграций и неймспейс для классов миграций в файле config/packages/doctrine_migrations.yaml

```shell
doctrine_migrations:  
    migrations_paths:  
        'App\Shared\Infrastructure\Database\Migrations': '%kernel.project_dir%/src/Shared/Infrastructure/Database/Migrations'  
    enable_profiler: false
```

4) В конфигурационном файле doctrine.yaml необходимо указать маппинг для модулей проекта

```shell
doctrine:  
    dbal:  
        url: '%env(resolve:DATABASE_URL)%'  
  
        # IMPORTANT: You MUST configure your server version,  
        # either here or in the DATABASE_URL env var (see .env file)        #server_version: '13'    orm:  
        auto_generate_proxy_classes: true  
        naming_strategy: doctrine.orm.naming_strategy.underscore_number_aware  
        auto_mapping: true  
        mappings:  
            Users:  
                is_bundle: false  
                type: xml  
                dir: '%kernel.project_dir%/src/Users/Infrastructure/Database/ORM'  
                prefix: 'App\Entity'  
                alias: Users
```

Указываем тип маппинга xml. Такой вариант поможет нам переложить ответственность за детали инфраструктурной реализации на соответствующий слой, тем самым избавив доменную область от лишних зависимостей.

Так же нужно закоментировать конфигурации тестовой и др баз данных, если все будет происходить в рамках одной БД.

5) Далее нужно определить докерфайл и конфигурации сервиса БД в docker-compose.yml

```Dockerfile
# Докерфайл для postgres
FROM postgres:15.0-alpine
```

```Dockerfile
# Postgres в докерфайле PHP
RUN apk add --no-cache libpq-dev && docker-php-ext-install pdo_pgsql
```

```yml
services:

...

  postgres:  
    container_name: clothing-db  
    build:  
      # Где искать докерфайл  
      context: ./postgres  
    ports:  
      - ${POSTGRES_PORT}:5432  
    environment:  
      POSTGRES_DB: ${POSTGRES_DB}  
      POSTGRES_USER: ${POSTGRES_USER}  
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}  
    volumes:  
      - db_data:/var/lib/postgresql/data:rw  
  
volumes:  
  db_data:
```

6) Указываем конфигурации в БД для подключения

Подключение в .env для подключения контейнеров (или приложения в зависисмости от того как делать его)
```env
POSTGRES_DB=clothing-db  
POSTGRES_PORT=5432  
POSTGRES_USER=apps  
POSTGRES_PASSWORD=apps
```

Подсключение в .env приложения
```env
DATABASE_URL="postgresql://apps:apps@clothing-db:5432/clothing-db?serverVersion=15&charset=utf8"
```


**Ключи:**
- [Symfony framework](Symfony-framework);
- [Doctrine ORM](Doctrine-ORM.md);

**Хештеги:** #Programming/Symfony/Doctrine