
# INSERT ONTO ... SELECT

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Модификация данных MySQL](mysql-modifying-data);

**Хештеги:** 
- #Programming/Databases/MySQL/Modify

## Синтаксис

Помимо использования значений строк в предложении `VALUES`, можно использовать результат инструкции `SELECT` в качестве источника данных для инструкции `INSERT`.

```sql
INSERT INTO table_name(column_list)
SELECT 
   select_list 
FROM 
   another_table
WHERE
   condition;
```

Необходимо учитывать, что количество колонок в `column_list` и `select_list` должно совпадать.

Выражение `INSERT INTO SELECT` может быть очень полезно, если для копирования данные из других таблиц в одну таблицу или суммирования данных из нескольких таблиц в одну.

Так же `table_name` и `another_table` могут быть одной и той же таблицей.

Каждое значение в VALUES может быть отдельнм запросом:

```sql
INSERT INTO stats(totalProduct, totalCustomer, totalOrder)
VALUES(
	(SELECT COUNT(*) FROM products),
	(SELECT COUNT(*) FROM customers),
	(SELECT COUNT(*) FROM orders)
);
```
