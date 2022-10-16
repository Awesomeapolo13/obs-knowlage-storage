docker run container_name - запускает контейнер (состояние running)

docker run --name container_name -d image_name - впервые запустит контейнер container_name на основе образа image_name
в фоновом режиме (-d)

docker rename container_old_name container_new_name - переименовывает ранее запущенный контейнер

docker stop container_name - останавливает контейнер (состояние stopped)

docker kill container_name - принудительно останавливает контейнер (не ждет завершения процессов в нем)

docker start container_name - запускает остановленный контейнер (возврщает в running)

docker restart container_name - перезапускает, работающий контейнер

docker pause container_name - ставит контейнер на паузу (состояние paused)

docker unpause container_name - снимает контейнер с паузы (возврщает в running)

docker create container_name (image_name) - создает новый контейнер (в состоянии stopped), можно передать образ для создания и имя контейнера вместе

docker rm container_name - удаляет остановленный контейнер (с флагом --force - остановит запущенный контейнер и потом его удалит)

docker ps - показывает список запущенных контейнеров со служебной информацией

docker ps -a - то же что команда выше, но покажет все остановленные
Выводят информацию:
- ID контейнера (CONTAINER ID)
- его образ (IMAGE)
- команда, приведшая к его созданию (COMMAND)
- когда создан (CREATED)
- его статус (STATUS)
- его порты (PORTS)
- его имя (NAMES)

docker container prune - удалит все остановленные контейнеры.

docker stats - статистика по всем запущенным контейнрам
Выводят информацию:
- ID контейнера (CONTAINER ID)
- его имя (NAME)
- загрузка ЦПУ (CPU %)
- сколько помяти использует / предельная память контейнера (MEM USAGE / LIMIT)
- сколько процентов используется от лимита по памяти (MEM %)
- сколько принимает сеть на вход/выход (NET I/O)
- (BLOCK I/O)
- идентификатор процесса (PID)


docker inspect container_name - получить служебную информацию о контейнере (возвращает json с этой информацией)

docker inspect -f "{{.State.Status}}" container_name - вытаскиваем служебную информацию о конкретном параметре контейнера


docker logs container_name - посмотреть логи контейнера container_name (для фильтрации использовать grep)

docker logs container_name | grep "11.857" -A 10 - вытащит логи удовлетворяющие фильтру и еще 10 строк после них
-B - то же самое только до найденной строки
-m 2 - выводит только две записи, удовлетворяющие фильтру
Так же в grep можно использовать регулярки.

docker logs container_name -f - флаг дает возможность следить за логами в реальном времени

docker logs container_name > test.txt - запишет результаты логирования в определенный файл

docker cp /path/to/local/file.txt container_name:/path/to/containner - копирование файла /path/to/local/file.txt в контейнер container_name по пути /path/to/containner

docker cp /home/aelog16/dir volume-9:/opt/app/data - копирование вольюма с локальной машины в контейнер

!! Для копирования из контейнера на локальную машины команда выполняется в обратном порядке.

**Do commands inside containers**

docker exec -options container_name command - выполнить комманду command внутри контейнера container_name
Опции:
-i - интерактивный режим
-t - псевдо tty (включает ssh внутри контейнера)
-d - запуск
-e - переменные окружения
-u - пользователь
-w - рабочая директория

Примеры
docker exec -e MYVAR=value container_name command
docker exec mongo mongo --version > mongo.txt - выводит результат работы команды в файл
!!!		   Тут одна команда  тут уже другая, выполняемая не внутри контейнера


docker exec -it container_name bash - войти в контейнер используя командную строку bashК чему относятся команды

docker exec -it container_name /bin/sh - аналогично команде выше, но используя другую среду

docker exec container_name bash -c 'several_commands > output.txt' - позволяет выполнить команды several_commands 
и записать их результат в output.txt, при этом файл создастся в контейнере