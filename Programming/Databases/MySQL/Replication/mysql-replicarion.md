# Репликация

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Репликация высоконагруженных систем](high-load-replication);
- Прямые:


**Хештеги:**
- #Programming/Databases/MySQL
- #Highload/Replication 

## Как работает

1) Помимо *WAL* данные пишутся в *binlogs*, формат одинаков для разных движков
2) Репликация осуществляется из *binlogs*
3) Есть два потока на реплике
	- Slave IO thread - читает данные;
	- Slave SQL thread - записывает в БД.

Чтение данных без репликации:

1) Приложение записано данные
2) Таблица на мастере
3) Приложение считало данные

Чтение данных с репликацией:

1) Приложение записано данные
2) Таблица на мастере
3) Binary log на мастере
4) Relay log на реплике
5) Из Relay log данные попадают на реплику
6) Приложение читают данные

## Типы реаликации

1) Row-based replication (RBR) - каждая строка таблицы передается отдельной командой.
	- проблема с командами - можно обновить что-то глобальным запросом (типа `update employee set salary = salary + 100`, т.е. будут проблемы с большими апдейтами)
2) Statement-based replication (SBR) - передаются целиком команды
	- проблемы с командами типа `update employee set salary = salary + 100` и `update employee set salary = salary + 100 where hired_at < unixtimestamp(now()) - 86400 * 365`;
	- если используем `now()`, то это будет разный `now()` для реплики и мастера, соответственно реплика перестанет быть копией т.к. там разные данные;

3) Mixed вариант - в общем случае используем SBR, для проблемных команд RBR.