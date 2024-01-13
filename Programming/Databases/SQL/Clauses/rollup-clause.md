
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

Чтобы проверить является ли NULL в результатах группировок промежуточным или полным итогом, необходимо использовать функцию `GROUPING()`. Она `GROUPING()` возвращает `1`, когда `NULL` встречается в строке суперагрегата, в противном случае она возвращает `0`. Функцию `GROUPING()` можно использовать в списке выбора, предложении `HAVING` и (начиная с MySQL 8.0.12) предложении `ORDER BY`.
## Примеры

`ROLLUP` генерирует строку промежуточного итога каждый раз, когда изменяется линия продуктов, и общий итог в конце результата. Поэтому у запроса ниже она будет иерархия `productLine > orderYear`.

```sql
SELECT 
    productLine, 
    orderYear,
    SUM(orderValue) totalOrderValue
FROM
    sales
GROUP BY 
    productline, 
    orderYear 
WITH ROLLUP;
```

Результат:

```shell
+------------------+-----------+-----------------+
| productLine      | orderYear | totalOrderValue |
+------------------+-----------+-----------------+
| Classic Cars     |      2003 |      1374832.22 |
| Classic Cars     |      2004 |      1763136.73 |
| Classic Cars     |      2005 |       715953.54 |
| Classic Cars     |      NULL |      3853922.49 |
| Motorcycles      |      2003 |       348909.24 |
| Motorcycles      |      2004 |       527243.84 |
| Motorcycles      |      2005 |       245273.04 |
| Motorcycles      |      NULL |      1121426.12 |
| Planes           |      2003 |       309784.20 |
| Planes           |      2004 |       471971.46 |
| Planes           |      2005 |       172881.88 |
| Planes           |      NULL |       954637.54 |
| Ships            |      2003 |       222182.08 |
| Ships            |      2004 |       337326.10 |
| Ships            |      2005 |       104490.16 |
| Ships            |      NULL |       663998.34 |
| Trains           |      2003 |        65822.05 |
| Trains           |      2004 |        96285.53 |
| Trains           |      2005 |        26425.34 |
| Trains           |      NULL |       188532.92 |
| Trucks and Buses |      2003 |       376657.12 |
| Trucks and Buses |      2004 |       465390.00 |
| Trucks and Buses |      2005 |       182066.45 |
| Trucks and Buses |      NULL |      1024113.57 |
| Vintage Cars     |      2003 |       619161.48 |
| Vintage Cars     |      2004 |       854551.85 |
| Vintage Cars     |      2005 |       323846.30 |
| Vintage Cars     |      NULL |      1797559.63 |
| NULL             |      NULL |      9604190.61 |
+------------------+-----------+-----------------+
29 rows in set (0.00 sec)
```

Если поменять местами порядок группировки, то нужно так же изменить из порядок в выражении `SELECT`. Результат изменится следующим образом:

```sql
+-----------+------------------+-----------------+
| orderYear | productLine      | totalOrderValue |
+-----------+------------------+-----------------+
|      2003 | Classic Cars     |      1374832.22 |
|      2003 | Motorcycles      |       348909.24 |
|      2003 | Planes           |       309784.20 |
|      2003 | Ships            |       222182.08 |
|      2003 | Trains           |        65822.05 |
|      2003 | Trucks and Buses |       376657.12 |
|      2003 | Vintage Cars     |       619161.48 |
|      2003 | NULL             |      3317348.39 |
|      2004 | Classic Cars     |      1763136.73 |
|      2004 | Motorcycles      |       527243.84 |
|      2004 | Planes           |       471971.46 |
|      2004 | Ships            |       337326.10 |
|      2004 | Trains           |        96285.53 |
|      2004 | Trucks and Buses |       465390.00 |
|      2004 | Vintage Cars     |       854551.85 |
|      2004 | NULL             |      4515905.51 |
|      2005 | Classic Cars     |       715953.54 |
|      2005 | Motorcycles      |       245273.04 |
|      2005 | Planes           |       172881.88 |
|      2005 | Ships            |       104490.16 |
|      2005 | Trains           |        26425.34 |
|      2005 | Trucks and Buses |       182066.45 |
|      2005 | Vintage Cars     |       323846.30 |
|      2005 | NULL             |      1770936.71 |
|      NULL | NULL             |      9604190.61 |
+-----------+------------------+-----------------+
25 rows in set (0.00 sec)
```

Теперь `ROLLUP` генерирует промежуточный итог каждый раз при смене года и общий итог в конце набора результатов.

При использовании функции `GROUPING()`:

```sql
SELECT 
    orderYear,
    productLine, 
    SUM(orderValue) totalOrderValue,
    GROUPING(orderYear),
    GROUPING(productLine)
FROM
    sales
GROUP BY 
    orderYear,
    productline
WITH ROLLUP;
```

Реузльтат:

```shell
+-----------+------------------+-----------------+---------------------+-----------------------+
| orderYear | productLine      | totalOrderValue | GROUPING(orderYear) | GROUPING(productLine) |
+-----------+------------------+-----------------+---------------------+-----------------------+
|      2003 | Classic Cars     |      1374832.22 |                   0 |                     0 |
|      2003 | Motorcycles      |       348909.24 |                   0 |                     0 |
|      2003 | Planes           |       309784.20 |                   0 |                     0 |
|      2003 | Ships            |       222182.08 |                   0 |                     0 |
|      2003 | Trains           |        65822.05 |                   0 |                     0 |
|      2003 | Trucks and Buses |       376657.12 |                   0 |                     0 |
|      2003 | Vintage Cars     |       619161.48 |                   0 |                     0 |
|      2003 | NULL             |      3317348.39 |                   0 |                     1 |
|      2004 | Classic Cars     |      1763136.73 |                   0 |                     0 |
|      2004 | Motorcycles      |       527243.84 |                   0 |                     0 |
|      2004 | Planes           |       471971.46 |                   0 |                     0 |
|      2004 | Ships            |       337326.10 |                   0 |                     0 |
|      2004 | Trains           |        96285.53 |                   0 |                     0 |
|      2004 | Trucks and Buses |       465390.00 |                   0 |                     0 |
|      2004 | Vintage Cars     |       854551.85 |                   0 |                     0 |
|      2004 | NULL             |      4515905.51 |                   0 |                     1 |
|      2005 | Classic Cars     |       715953.54 |                   0 |                     0 |
|      2005 | Motorcycles      |       245273.04 |                   0 |                     0 |
|      2005 | Planes           |       172881.88 |                   0 |                     0 |
|      2005 | Ships            |       104490.16 |                   0 |                     0 |
|      2005 | Trains           |        26425.34 |                   0 |                     0 |
|      2005 | Trucks and Buses |       182066.45 |                   0 |                     0 |
|      2005 | Vintage Cars     |       323846.30 |                   0 |                     0 |
|      2005 | NULL             |      1770936.71 |                   0 |                     1 |
|      NULL | NULL             |      9604190.61 |                   1 |                     1 |
+-----------+------------------+-----------------+---------------------+-----------------------+
25 rows in set (0.00 sec)
```