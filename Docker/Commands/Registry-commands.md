
docker pull img_name - скачивает образ img_name на локальную машину

docker-compose pull - скачивает образы из файла docker-compose на локальную машину

docker search img_name - ищет в локальном реджистри образы, содержащие в названии строку img_name

docker search --no-trunc img_name - то же что и выше, но текст в столбце с описанием не будет обрезаться

docker push img_name - отправить собранный, измененный образ в удаленный реджистри