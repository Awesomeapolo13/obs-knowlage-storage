
# GROUP BY

**Ключи:**
- Обратные:
	- [Выражения SQL](sql-clause);
- Прямые:
	- [HAVING](having-clause);
	- [HAVING COUNT](having-count-clause);


**Хештеги:** #Programming/Databases/SQL/Clause

## Определение

Предложение `GROUP BY` группирует набор строк в набор итоговых строк на основе значений столбцов или выражений. Он возвращает одну строку для каждой группы и уменьшает количество строк в наборе результатов.

```sql
SELECT 
    c1, c2,..., cn, aggregate_function(ci)
FROM
    table_name
WHERE
    conditions
GROUP BY c1 , c2,...,cn;
```

На практике `GROUP BY` часто используется с агрегирующим функциями, такими как `SUM`, `AVG`, `MAX`, `MIN` и `COUNT`. Агрегирующая функция, которая появляется в предложении `SELECT`, предоставляет информацию для каждой группы.

При работе с запросами без агрегирующих функций `GROUP BY` работает как `DISTINCT`.

Стандарт SQL запрещает использование алиасов в выражении `GROUP BY`, однако в MySQL такое поведение разрешено. 

## Примеры

1) Помимо столбцов, вы можете группировать строки по выражениям. Следующий запрос вычисляет общий объем продаж за каждый год:

```sql
SELECT 
  YEAR(orderDate) AS year, 
  SUM(quantityOrdered * priceEach) AS total 
FROM 
  orders 
  INNER JOIN orderdetails USING (orderNumber) 
WHERE 
  status = 'Shipped' 
GROUP BY 
  YEAR(orderDate);
```

```shell
+------+------------+
| year | total      |
+------+------------+
| 2003 | 3223095.80 |
| 2004 | 4300602.99 |
| 2005 | 1341395.85 |
+------+------------+
3 rows in set (0.02 sec)
```

В этом примере мы использовали функцию `YEAR` для извлечения данных за год из даты заказа (orderDate) и включили в общий объем продаж только заказы со статусом отгрузки.

Обратите внимание, что выражение в предложении `SELECT` должно совпадать с выражением в предложении `GROUP BY`.

2) Для фильтрации результатов `GROUP BY` можно использовать выражение `HAVING`. В следующем запросе используется предложение `HAVING` для выбора общего объема продаж за годы после 2003 года.

```sql
SELECT 
  YEAR(orderDate) AS year, 
  SUM(quantityOrdered * priceEach) AS total 
FROM 
  orders 
  INNER JOIN orderdetails USING (orderNumber) 
WHERE 
  status = 'Shipped' 
GROUP BY 
  year 
HAVING 
  year > 2003;

```

3) Группировка множества строк. Следующий запрос возвращает год, статус заказа и общую сумму заказа для каждой комбинации года и статуса заказа, группируя строки в группы:

```sql
SELECT 
  YEAR(orderDate) AS year, 
  status, 
  SUM(quantityOrdered * priceEach) AS total 
FROM 
  orders 
  INNER JOIN orderdetails USING (orderNumber) 
GROUP BY 
  year, 
  status 
ORDER BY 
  year;
```

```shell
+------+------------+------------+
| year | status     | total      |
+------+------------+------------+
| 2003 | Cancelled  |   67130.69 |
| 2003 | Resolved   |   27121.90 |
| 2003 | Shipped    | 3223095.80 |
| 2004 | Cancelled  |  171723.49 |
| 2004 | On Hold    |   23014.17 |
| 2004 | Resolved   |   20564.86 |
| 2004 | Shipped    | 4300602.99 |
| 2005 | Disputed   |   61158.78 |
| 2005 | In Process |  135271.52 |
| 2005 | On Hold    |  146561.44 |
| 2005 | Resolved   |   86549.12 |
| 2005 | Shipped    | 1341395.85 |
+------+------------+------------+
12 rows in set (0.01 sec)
```

## Примечания

Из MySQL 8.0 или более поздние версии удалили неявную сортировку для предложения `GROUP BY`. Таким образом, если при использовании более ранние версии, можно обнаружить, что набор результатов с предложением `GROUP BY` отсортирован.