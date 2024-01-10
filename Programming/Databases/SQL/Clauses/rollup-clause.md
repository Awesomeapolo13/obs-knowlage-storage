
# ROLLUP

**Ключи:**
- Обратные:
	- [Выражения SQL](sql-clause);
- Прямые:
	- [GROUP BY](group-by-clause);


**Хештеги:** #Programming/Databases/SQL/Clause

## Определение

`ROLLUP` генерирует несколько наборов групп на основе столбцов или выражений, указанных в предложении `GROUP BY`.

```sql
SELECT 
    productLine, 
    SUM(orderValue) totalOrderValue
FROM
    sales
GROUP BY 
    productline WITH ROLLUP;
```

Результат:

```shell
+------------------+-----------------+
| productLine      | totalOrderValue |
+------------------+-----------------+
| Classic Cars     |      3853922.49 |
| Motorcycles      |      1121426.12 |
| Planes           |       954637.54 |
| Ships            |       663998.34 |
| Trains           |       188532.92 |
| Trucks and Buses |      1024113.57 |
| Vintage Cars     |      1797559.63 |
| NULL             |      9604190.61 |
+------------------+-----------------+
8 rows in set (0.00 sec)
```

Если в предложении `GROUP BY` указано более одного столбца, предложение `ROLLUP` будет предполагать иерархию среди входных столбцов.

Например:

```sql
GROUP BY c1, c2, c3 WITH ROLLUP
```

Тут будет сформирована следующая иерархия:

```shell
c1 > c2 > c3
```

Она определит следующий набор группировок:

```shell
(c1, c2, c3)
(c1, c2)
(c1)
()
```

Предложение `HAVING` применяет условие к группам строк, а предложение `WHERE` применяет условие к отдельным строкам.

Если вы опустите выражение `GROUP BY`, предложение `HAVING` будет вести себя как выражение `WHERE`.

*Примечание:* стандарт SQL определяет, что `HAVING` оценивается перед предложением `SELECT` и после предложения `GROUP BY`. Однако, в MySQL, предложение `HAVING` после предложений `FROM`, `WHERE` и `GROUP BY`.
