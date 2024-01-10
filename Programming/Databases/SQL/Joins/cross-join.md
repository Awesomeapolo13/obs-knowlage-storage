
# CROSS JOIN

**Ключи:**
- Обратные:
	- [Joins](joins);
- Прямые:

**Хештеги:** #Programming/Databases/SQL/Join

## Определение

`CROSS JOIN`, в отличие от  `RIGHT JOIN` и `LEFT JOIN`, не имеет условия присоединения. 

`CROSS JOIN` создает *декартово произведение* строк из объединенных таблиц. `CROSS JOIN` объединяет каждую строку из первой таблицы с каждой строкой из правой таблицы для создания набора результатов.

Предположим, что в первой таблице *n* строк, а во второй таблице *m* строк. Перекрестное соединение, объединяющее таблицы, вернет `nxm` строк.

```sql
SELECT select_list
FROM table_1
CROSS JOIN table_2;
```

`CROSS JOIN` полезен для создания данных планирования. Например, вы можете выполнить планирование продаж, используя перекрестное объединение клиентов, продуктов и лет.

## Пример

В этом примере используется предложение перекрестного соединения для объединения участников с таблицами комитетов:

```sql
SELECT 
    m.member_id, 
    m.name AS member, 
    c.committee_id, 
    c.name AS committee
FROM
    members m
CROSS JOIN committees c;
```

Результат:

```shell
+-----------+--------+--------------+-----------+
| member_id | member | committee_id | committee |
+-----------+--------+--------------+-----------+
|         1 | John   |            4 | Joe       |
|         1 | John   |            3 | Amelia    |
|         1 | John   |            2 | Mary      |
|         1 | John   |            1 | John      |
|         2 | Jane   |            4 | Joe       |
|         2 | Jane   |            3 | Amelia    |
|         2 | Jane   |            2 | Mary      |
|         2 | Jane   |            1 | John      |
|         3 | Mary   |            4 | Joe       |
|         3 | Mary   |            3 | Amelia    |
|         3 | Mary   |            2 | Mary      |
|         3 | Mary   |            1 | John      |
|         4 | David  |            4 | Joe       |
|         4 | David  |            3 | Amelia    |
|         4 | David  |            2 | Mary      |
|         4 | David  |            1 | John      |
|         5 | Amelia |            4 | Joe       |
|         5 | Amelia |            3 | Amelia    |
|         5 | Amelia |            2 | Mary      |
|         5 | Amelia |            1 | John      |
+-----------+--------+--------------+-----------+
20 rows in set (0.00 sec)
```
