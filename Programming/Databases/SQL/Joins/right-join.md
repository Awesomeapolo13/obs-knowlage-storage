
# RIGHT JOIN

**Ключи:**
- Обратные:
	- [Joins](joins);
- Прямые:
	- [LEFT JOIN](left-join);


**Хештеги:** #Programming/Databases/SQL/Join

## Определение

Соедиение с помощью `RIGHT JOIN` происходит аналогично `LEFT JOIN` , за исключением порядка обработки таблиц. Теперь начало выборки элементов в результирующую таблицу начинается с правой таблицы. Далее к правой таблице прибавляются колонки из левой. Для строк, где условие присоединения было удовлетворено, колонки левой таблицы заполняются соответствующими значениями, где не удовлетворено - значениями `NULL`.

```sql
SELECT column_list 
FROM table_1 
RIGHT JOIN table_2 ON join_condition;
```

![[Pasted image 20240110112004.png]]

Так же можно использовать `RGHT JOIN`, чтобы найти записи правой таблицы, у которых нет соответствующих строк в левой. Пример запроса и диаграммы Венна ниже:

```sql
SELECT 
    m.member_id, 
    m.name AS member, 
    c.committee_id, 
    c.name AS committee
FROM
    members m
RIGHT JOIN committees c USING(name)
WHERE m.member_id IS NULL;
```

![[Pasted image 20240110112113.png]]