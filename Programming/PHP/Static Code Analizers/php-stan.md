
# PHP-stan

PHPStan - PHP Static Analysis Tool. Позволяет выявлять ошибки еще до запуска кода интерпретатором. 


## Установа и настройка

```shell
composer require phpstan/phpstan --dev
```

Добавим в корне файл конфигурации phpstan.neon.

## Использование

Заполняем конфигруационный файл phpstan.neon

```neon
parameters:  
#    Уровень строгости проверки  
    level: 6  
#    Параметр для решения проблем с анализом дженериков  
    checkGenericClassInNonGenericObjectType: false
```

Запускаем анализ

```shell
vendor/bin/phpstan analyse src tests\
```

## Настройка в PHPStorm

Среда разработки PHPStorm предлагает возможность отслеживать ошибок посредством phpstan. Для настройки PHPStorm необходимо.
- перейтив настройки IDE PHP -> Quality tools -> PHPStan;
- нажимаем на  ... и создаем конфигурацию;
- выбираем интерпретатор;
- настраиваем маппинг и нажимаем на кнопку валидации;
- указываем уровень ошибок и включаем отслеживание.

**Ключи:**
- [Статические анализаторы кода](static-code-analizers);

**Хештеги:** #Programming/PHP/CodeAnalizer