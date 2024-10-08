
# MERGE

**Ключи:**
- Обратные:
	- [Работа с таблицами](mysql-table-management);
	- [Механизмы хранения](mysql-storage-engine);
- Прямые:

**Хештеги:** #Programming/Databases/MySQL/Tables/Engine


## Определение

Таблица MERGE – это виртуальная таблица, объединяющая несколько таблиц MyISAM, имеющих ту же структуру, что и одна таблица. Механизм хранения MERGE также известен как механизм MRG_MyISAM. Таблицы MERGE не имеют индексов. Вместо этого они используют индексы таблиц компонентов.

Если вы используете оператор DROP TABLE для таблицы MERGE, MySQL удаляет только таблицу MERGE и не удаляет базовые таблицы.

Таблицы MERGE позволяют повысить производительность при объединении нескольких таблиц. MySQL позволяет выполнять только операции SELECT, DELETE, UPDATE и INSERT с таблицами MERGE.
