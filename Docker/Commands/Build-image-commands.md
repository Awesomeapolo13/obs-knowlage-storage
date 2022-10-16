docker build - собирает образ для контейнера
-f - собирает проект в соответствии с указанным после опции Dockerfile
-q - указать что нам не нужне output
-t - установить tag для нашего образа

docker build -f ./apps/api/Dockerfile -t test:latest . - произвести сборку образа по докерфайлу ./apps/api/Dockerfile, 
образу назначить тег (имя и версию) test:latest, контекст для сборки (т.е. файлы и директории) брать из текущей папки (обозначено как .)

docker run -d --name container_name test:latest - запустить контейнер container_name в фоновом режиме (-d) на основе образа test:latest

docker run -d --name container_name --network network_name -d image:latest - запустить контейнер container_name в фоновом режиме (-d) на основе образа test:latest с подключением к сети network_name

docker run --name=container_name -p 3000:3000 --network network_name -d image:latest - запустить контейнер container_name в фоновом режиме (-d) на основе образа test:latest с подключением к сети network_name и настроить доступ извне к этому контейнеру через порт 3000