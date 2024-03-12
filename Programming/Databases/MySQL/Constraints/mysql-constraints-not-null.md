
# NOT NULL

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Ограничения](mysql-constraints);
- Прямые:

**Хештеги:** #Programming/Databases/MySQL/Constraints 

## Определение

**Ограничение NOT NULL** — механизм, который обеспечивает, что значение в колонке не будет содержать `NULL`. Столбец может иметь только одно ограничение NOT NULL, которое обеспечивает соблюдение правила, согласно которому столбец не должен содержать значений `NULL`. Другими словами, при попытке обновить или вставить значение `NULL` в столбец `NOT NULL`, MySQL выдаст ошибку.

Хорошей практикой является наличие ограничения `NOT NULL` в каждом столбце таблицы, даже если на это нет особых причин.

Это связано с тем, что значения `NULL` могут усложнить запросы, поскольку для их обработки необходимо использовать функции, связанные с `NULL`, такие как `ISNULL()`, `IFNULL()` и `NULLIF()`.
## Синтаксис

Вот основной синтаксис установки ограничения `NOT NULL`:

```sql
column_name data_type NOT NULL;
```

Для добавления ограничения в уже существующую таблицу:

```sql
ALTER TABLE table_name
CHANGE 
   old_column_name 
   new_column_name column_definition;
```

Например:

```sql
ALTER TABLE tasks 
CHANGE 
    end_date 
    end_date DATE NOT NULL;
```

Чтобы удалить ограничение:

```sql
ALTER TABLE table_name
MODIFY column_name column_definition;
```

Например:

```sql
ALTER TABLE tasks 
MODIFY end_date DATE;
```
