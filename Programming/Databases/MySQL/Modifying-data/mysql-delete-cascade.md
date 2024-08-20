
# ON CASCADE DELETE

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Модификация данных MySQL](mysql-modifying-data);
	- [DELETE](mysql-delete);
- Прямые:
	
**Хештеги:** 
- #Programming/Databases/MySQL/Modify

## Определение

Внешний ключ предоставляет наиболее эффективный путь автоматического удаления данных из дочерних таблиц, при удалении записей в родительских.

Ограничение внешнего ключа создается в дочерней таблице. Ниже приведены примеры DDL запросов для создания таблиц зданий и комнат:

```sql
CREATE TABLE buildings (
    building_no INT PRIMARY KEY AUTO_INCREMENT,
    building_name VARCHAR(255) NOT NULL,
    address VARCHAR(255) NOT NULL
);

CREATE TABLE rooms (
    room_no INT PRIMARY KEY AUTO_INCREMENT,
    room_name VARCHAR(255) NOT NULL,
    building_no INT NOT NULL,
    FOREIGN KEY (building_no)
        REFERENCES buildings (building_no)
        ON DELETE CASCADE
);
```

## Советы для работы со связями

Чтобы узнать какие таблицы связаны орграничениями `ON DELETE CASCADE`, нужно выполнить запрос ниже к БД `information_schema`:

```sql

USE information_schema;

SELECT 
    table_name
FROM
    referential_constraints
WHERE
    constraint_schema = 'database_name'
        AND referenced_table_name = 'parent_table'
        AND delete_rule = 'CASCADE'
```
