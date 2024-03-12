
# UNIQUE

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Ограничения](mysql-constraints);
- Прямые:

**Хештеги:** #Programming/Databases/MySQL/Constraints 

## Определение

**Ограничение UNIQUE** — это ограничение целостности, обеспечивающее уникальность значений в колонке или группе колонок таблицы. Ограничение `UNIQUE` может быть ограничением столбца или ограничением таблицы.

## Синтаксис

Для определения ограничения `UNIQUE` в колонке при создании таблицы:

```sql
CREATE TABLE table_name(
    ...,
    column1 datatype UNIQUE,
    ...
);
```

Если же нужно установить это ограничение на группу колонок:

```sql
CREATE TABLE table_name(
   ...
   column1 datatype,
   column2 datatype,
   ...,
   UNIQUE(column1, column2)
);
```

Если определить ограничение `UNIQUE` без указания имени, MySQL автоматически генерирует для него имя. Чтобы определить ограничение `UNIQUE` с кастомным именем, существует следующий синтаксис:

```sql
[CONSTRAINT constraint_name]
UNIQUE(column_list)
```

В этом синтаксисе имя ограничения `UNIQUE` указывается после ключевого слова `CONSTRAINT`.

Для удаления ограничения `UNIQUE` можно использовать выражения `DROP INDEX` или `ALTER TABLE`:

```sql
DROP INDEX index_name ON table_name;

ALTER TABLE table_name
DROP INDEX index_name;
```

Для добавления нового ограничения `UNIQUE`:

```sql
ALTER TABLE table_name
ADD CONSTRAINT constraint_name 
UNIQUE (column_list);
```

## UNIQUE и NULL

В MySQL значения `NULL` рассматриваются как отдельные, когда речь идет об ограничениях уникальности. Таким образом, если есть столбец, который принимает значения `NULL`, вы можете вставить в него несколько значений.

Например, у нас есть таблица ниже.

```sql
CREATE TABLE contacts(
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    phone VARCHAR(20) UNIQUE
)
```

Если мы добавим в таблицу `contacts` несколько пользователей с телефонным номером `NULL`, никакой ошибки уникальности значений это не вызовет.

## UNIQUE и индексы

При определении ограничения уникальности для столбца или группы столбцов, MySQL создает соответствующий индекс `UNIQUE` и использует этот индекс для обеспечения соблюдения правила.
В этом можно убедиться, использовав выражение ниже:

```sql
SHOW INDEX FROM table_name;
```

На практике удаление первичного ключа довольно редко применяется. Однако если необходимо это сделать, можно использовать выражение `ALTER TABLE ... DROP PRIMARY KEY`:

```sql
ALTER TABLE table_name
DROP PRIMARY KEY;
```
