
# Построение образа

**Ключи:**
- Обратные:
	- [Все команды](All-docker-commands);
- Прямые:

**Хештеги:**
- #Docker/Command 
- #Docker/Image

1) Собрать образ для контейнера:

```shell
docker build
```

`-f` - собирает проект в соответствии с указанным после опции `Dockerfile`;
`-q` - указать что нам не нужне output;
`-t` - установить tag для нашего образа.

2)  Произвести сборку образа по докерфайлу ./apps/api/Dockerfile:

```shell
docker build -f ./apps/api/Dockerfile -t test:latest
```

Образу необходимо назначить тег (имя и версию) `test:latest`, контекст для сборки (т.е. файлы и директории) брать из текущей папки (обозначено как .).

3) Запустить контейнер `container_name` в фоновом режиме (`-d`) на основе образа `test:latest`

```shell
docker run -d --name container_name test:latest
```

4) Запустить контейнер `container_name` в фоновом режиме (`-d`) на основе образа `test:latest` с подключением к сети `network_name`:

```shell
docker run -d --name container_name --network network_name -d image:latest
```

5) Запустить контейнер container_name в фоновом режиме (-d) на основе образа test:latest с подключением к сети network_name и настроить доступ извне к этому контейнеру через порт 3000:

```shell
docker run --name=container_name -p 3000:3000 --network network_name -d image:latest
```
