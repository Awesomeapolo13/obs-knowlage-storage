
# Команды PHP, для выполнения в теримнале

**Связи:**
- Обратные
	- [PHP](PHP);
- Прямые:


**Хештеги:** #Programming/PHP/TerminalCommands

1) Показывает список подключенных в php.ini модулей

```shell
php -m
```

Пример вывода:
```shell
[PHP Modules]
Core
ctype
curl
date
dom
fileinfo
filter
ftp
hash
iconv
json
libxml
mbstring
mysqlnd
openssl
pcre
PDO
pdo_sqlite
Phar
posix
readline
Reflection
session
SimpleXML
sodium
SPL
sqlite3
standard
tokenizer
xdebug
xml
xmlreader
xmlwriter
zlib

[Zend Modules]
Xdebug
```

2) Перевыбрать версию php

```shell
sudo update-alternatives --config php
```

Пример вывода, вводим цифру из блока Selection, чтобы выбрать другую доступную версию:

```shell
There are 3 choices for the alternative php (providing /usr/bin/php).

  Selection    Path                  Priority   Status
------------------------------------------------------------
* 0            /usr/bin/php.default   100       auto mode
  1            /usr/bin/php.default   100       manual mode
  2            /usr/bin/php8.1        81        manual mode
  3            /usr/bin/php8.2        82        manual mode

Press <enter> to keep the current choice[*], or type selection number: 3
update-alternatives: using /usr/bin/php8.2 to provide /usr/bin/php (php) in manual mode
```

