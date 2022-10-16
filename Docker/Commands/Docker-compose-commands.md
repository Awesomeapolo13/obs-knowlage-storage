
docker-compose --env-file .env config - команда выводит конфигурацию docker-compose с учетом переменных из .env

docker-compose -f docker-compose -f docker-compose.yml -f docker-compose.dev.yml up -d - запускает контейнеры с учетом конфигурация в обоих файлах, при этом подразумевается что docker-compose.yml содержит оновную конфигруацию, а docker-compose.dev.yml дополнения для dev окружения (например, порты)