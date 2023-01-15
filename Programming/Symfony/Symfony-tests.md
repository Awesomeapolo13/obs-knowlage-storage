
# Тестирование приложений с Symfony

Установка пакетов для тестировния:

```shell
composer require --dev phpunit/phpunit symfony/test-pack
```

После установки появляются дополнительные файлы конфигурации phpunit.
Переименовать файл phpunit.xml.dist в phpunit.xml

Настроим php storm для работы с тестовым фреймворком.

1) Открываем *File->Settings*
2) Заходим в раздел *PHP->Test Framework*s
3) Нажать + и добавить тип конфигурации *PHPUnit by Remote Interpreter*
4) Выбрать контейнер с удаленным интерпретатором


**Хештеги:** #Programming/Symfony/Test