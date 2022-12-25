
#  CS-fixer

Рекомендуется документацией симфони как основной инструмент для поддержания единого код стайла в проекте.

## Установа и настройка

Установить:

```shell
composer require friendsofphp/php-cs-fixer --dev
```

После установки по умолчанию в корне создается конфигурация cs-fixer

```php
<?php  
  
$finder = (new PhpCsFixer\Finder())  
    ->in(__DIR__)
    // исключаем директории для сканирования на код стайл
    ->exclude('var')  
;  
  
return (new PhpCsFixer\Config())
	// Установка набора правил для сканирования
    ->setRules([  
	    // Правила свойственные Symfony
        '@Symfony' => true,  
    ])  
    ->setFinder($finder)  
;
```

По умолчанию фиксер сканирует все директории в папке проекта, за исключением var. Чтобы расширить директории свободные от сканирования зададим методу ->exclude в виде аргумента список строк - имен директорий.
Например, добавим vendor.

Чтобы добавить правило, нужно найти его в документации фиксера. и затем включить в метод setRules. Например, так выглядит добавления правила на разрешение использования snake_case.

```php
return (new PhpCsFixer\Config())
	// Установка набора правил для сканирования
    ->setRules([  
	    // Правила свойственные Symfony
        '@Symfony' => true,
        'php_unit_method_casing' => [  
		    'case' => 'snake_case'  
		]
    ])
```

## Работа

Есть две команды для работы с фиксером. Одна из них обнаруживает несоответствия, другая, автоматически исправляет их.

1) Показывает несоответствия, но не правит их автоматически (благодаря флагу --dry-run)

```shell
php vendor/bin/php-cs-fixer fix --dry-run --diff
```

Пример результата выполнения:

```shell
Loaded config default from "/var/www/.php-cs-fixer.dist.php".
Using cache file ".php-cs-fixer.cache".
   1) src/Shared/Application/Command/CommandInterface.php
      ---------- begin diff ----------
--- /var/www/src/Shared/Application/Command/CommandInterface.php
+++ /var/www/src/Shared/Application/Command/CommandInterface.php
@@ -6,5 +6,4 @@
 
 interface CommandInterface
 {
-
 }

      ----------- end diff -----------

   2) src/Shared/Application/Command/CommandHandlerInterface.php
      ---------- begin diff ----------
--- /var/www/src/Shared/Application/Command/CommandHandlerInterface.php
+++ /var/www/src/Shared/Application/Command/CommandHandlerInterface.php
@@ -6,5 +6,4 @@
 
 interface CommandHandlerInterface
 {
-
 }

      ----------- end diff -----------


```

2) Автоматически правит все ошибки с код стайлом

```shell
php vendor/bin/php-cs-fixer fix
```

## PHP Storm

Среда разработки PHPStorm предлагает возможность отслеживать несоответсвия посредством себя. Для настройки PHPStorm необходимо.
- перейтив настройки IDE PHP -> Quality tools -> PHP CS Fixer;
- нажимаем на  ... и создаем конфигурацию;
- выбираем интерпретатор;
- настраиваем маппинг и нажимаем на кнопку валидации;
- далее нужно настроить инспекцию ошибок, выбрав их тип.

**Ключи:**
- [Статические анализаторы кода](static-code-analizers);

**Хештеги:** #Programming/PHP/CodeAnalizer