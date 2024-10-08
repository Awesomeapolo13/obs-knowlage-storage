
# Механизы хранения MySQL

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Работа с таблицами](mysql-table-management)
- Прямые:
	- [InnoDB](mysql-innodb-engine);
	- [MyISAM](mysql-myisam-engine);
	- [Archive](mysql-archive-engine);
	- [MERGE](mysql-merge-engine);
	- [CSV](mysql-csv-engine);
	- [BLACKHOLE](mysql-blackhole-engine);
	- [FEDERATED](mysql-federated-engine);
	- [Memory](mysql-memory-engine);

**Хештеги:** #Programming/Databases/MySQL/Tables/Engine

## Определение

В MySQL механизм хранения (*storage engine*) — это программный компонент, отвечающий за управление хранением, извлечением и манипулированием данными в таблицах. Механизм хранения также определяет базовую структуру и функции таблиц.

MySQL поддерживает несколько механизмов хранения, каждый из которых имеет свой набор функций. Чтобы найти доступные механизмы хранения на сервере MySQL, можно использовать следующий запрос:

```sql
SELECT 
  engine, 
  support 
FROM 
  information_schema.engines 
ORDER BY 
  engine;
```

Результатом будет таблица механизмов хранения с колонкой, указывающей на их поддержку (`YES` - поддерживает, `NO` - не поддерживает, `DEFAULT` - используется по умолчанию):

```shell
+--------------------+---------+
| engine             | support |
+--------------------+---------+
| ARCHIVE            | YES     |
| BLACKHOLE          | YES     |
| CSV                | YES     |
| FEDERATED          | NO      |
| InnoDB             | DEFAULT |
| MEMORY             | YES     |
| MRG_MYISAM         | YES     |
| MyISAM             | YES     |
| ndbcluster         | NO      |
| ndbinfo            | NO      |
| PERFORMANCE_SCHEMA | YES     |
+--------------------+---------+
11 rows in set (0.02 sec)

```

Другой способ ввести команду `SHOW ENGINES`. Результатом будет расширенная таблица с информацией по каждому движку.

Чтобы выбрать специфический движок при создании таблица, необходимо указать выражение `ENGINE`. При этом, если опустить это выражение, то MySQL выберет механизм хранения по умолчанию.

```sql
CREATE TABLE table_name(
    column_list
) ENGINE = engine_name;
```

Таблицы сравнения возможностей механизмов хранения MySQL.

![[Pasted image 20240113144129.png]]