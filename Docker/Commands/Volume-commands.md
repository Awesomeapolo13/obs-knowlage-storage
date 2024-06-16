
`docker volume ls` - показать список вольюмсов

`dokcer volume create name` - создать вольюм с именем name

`docker inspect name` - получить служебную информацию о вольюме name

`docker run --name volume-1 -d -v demo:/opt/app/data demo4:latest `- запустить контейнера volume-1 в фоновом режиме на основании контейнера demo4, писать с него данные в вольюм demo:/opt/app/data.