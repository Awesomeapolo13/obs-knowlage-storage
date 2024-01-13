
# Подзапросы

**Ключи:**
- Обратные:
	- [SQL](SQL);
- Прямые:
	- [Производные таблицы](sql-derived-table);
	- [Оператор EXISTS](exists-operator);

**Хештеги:** #Programming/Databases/SQL

## Определение

**Подзапрос MySQL** — это запрос, вложенный в другой запрос, например в `SELECT`, `INSERT`, `UPDATE` или `DELETE`. Кроме того, подзапрос может быть вложен в другой подзапрос.

Подзапрос MySQL называется *внутренним* запросом, тогда как запрос, содержащий подзапрос, называется *внешним* запросом. Подзапрос можно использовать везде, где используется выражение, и он должен быть заключен в круглые скобки.

```sql
SELECT 
    lastName, firstName
FROM
    employees
WHERE
    officeCode IN (
    SELECT 
            officeCode
        FROM
            offices
        WHERE
            country = 'USA'
    );
```