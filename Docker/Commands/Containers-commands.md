
# Контейнеры

**Ключи:**
- Обратные:
	- [Все команды](All-docker-commands);

**Хештеги:**
- #Docker/Command
- #Docker/Container


## Общие команды

1) Запустить контейнер (состояние `running`)

```shell
docker run container_name
```
2) Впервые запустить контейнер `container_name` на основе образа `image_name` в фоновом режиме (`-d`)

```shell
docker run --name container_name -d image_name
```

3) Переименовать ранее запущенный контейнер

```shell
docker rename container_old_name container_new_name
```

4) Останавливает контейнер (состояние `stopped`)

```shell
docker stop container_name
```

5) Принудительно остановить контейнер (не ждет завершения процессов в нем)

```shell
docker kill container_name
```

6) Запустить остановленный контейнер (возврщает в `running`)

```shell
docker start container_name
```

7) Перезапустить работающий контейнер

```shell
docker restart container_name 
```

8) Поставить контейнер на паузу (состояние `paused`)

```shell
docker pause container_name
```

9) Снять контейнер с паузы (возврщает в `running`)
```shell
docker unpause container_name
```

10) Создать новый контейнер (в состоянии `stopped`), можно передать образ для создания и имя контейнера вместе

```shell
docker create container_name (image_name)
``` 

11) Удалить *остановленный* контейнер (с флагом `--force` - остановит запущенный контейнер и потом его удалит)

```shell
docker rm container_name
```

12) Показать список запущенных контейнеров со служебной информацией

```shell
docker ps
```

13) Показать список всех остановленных контейнеров со служебной информацией

```shell
docker ps -a
```

Команды в п.12 и п.13 выводят следующую информацию:
- ID контейнера (CONTAINER ID)
- его образ (IMAGE)
- команда, приведшая к его созданию (COMMAND)
- когда создан (CREATED)
- его статус (STATUS)
- его порты (PORTS)
- его имя (NAMES)

14) Удаление всех остановленных контейнеров

```shell
docker container prune
```

15) Статистика по всем запущенным контейнерам.

```shell
docker stats
```

Выводят информацию:
- ID контейнера (CONTAINER ID)
- его имя (NAME)
- загрузка ЦПУ (CPU %)
- сколько помяти использует / предельная память контейнера (MEM USAGE / LIMIT)
- сколько процентов используется от лимита по памяти (MEM %)
- сколько принимает сеть на вход/выход (NET I/O)
- (BLOCK I/O)
- идентификатор процесса (PID)

16) Получить служебную информацию о контейнере (возвращает `json` с этой информацией)

```shell
docker inspect container_name
```

17) Показывает служебную информацию о конкретном параметре контейнера

```shell
docker inspect -f "{{.State.Status}}" container_name
```


## Логи контейнера

1) Посмотреть логи контейнера `container_name` (для фильтрации использовать grep)

```shell
docker logs container_name 
```

2) Показать логи удовлетворяющие фильтру и еще 10 строк после них

```shell
docker logs container_name | grep "11.857" -A 10
```


-B - то же самое только до найденной строки
-m 2 - выводит только две записи, удовлетворяющие фильтру
Так же в grep можно использовать регулярки.

3) Флаг `-f` дает возможность следить за логами в реальном времени

```shell
docker logs container_name -f
```

4) Запишет результаты логирования в определенный файл

```shell
docker logs container_name > test.txt
```

## Копирование в контейнер

1) Копирование файла `/path/to/local/file.txt` в контейнер `container_name` по пути `/path/to/containner`:

```shell
docker cp /path/to/local/file.txt container_name:/path/to/containner
```

2) Копирование вольюма с локальной машины в контейнер

```shell
docker cp /home/aelog16/dir volume-9:/opt/app/data
```

*!! Для копирования из контейнера на локальную машины команда выполняется в обратном порядке.*

## Выполнение команд внутри контейнера

1) Выполнить комманду `command` внутри контейнера `container_name`

```shell
docker exec -options container_name command
```

Опции:
-i - интерактивный режим
-t - псевдо tty (включает ssh внутри контейнера)
-d - запуск
-e - переменные окружения
-u - пользователь
-w - рабочая директория

*Примеры:*
```shell
docker exec -e MYVAR=value container_name command
```

2) Выводит результат работы команды в файл:

```shell
docker exec mongo mongo --version > mongo.txt
```

*!!!	Тут одна команда  тут уже другая, выполняемая не внутри контейнера*

3)  Войти в контейнер, используя командную строку bash

```shell
docker exec -it container_name bash
```

4) Аналогично команде выше, но используя другую среду (`/bin/sh`)

```shell
docker exec -it container_name /bin/sh
```

5) Позволяет выполнить команды `several_commands` и записать их результат в `output.txt`, при этом файл создастся в контейнере

```shell
docker exec container_name bash -c 'several_commands > output.txt' 
```
