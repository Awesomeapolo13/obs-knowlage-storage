# Производные таблицы

**Ключи:**
- Обратные:
	- [SQL](SQL);
	- [Подзапрос](sql-subquery);
- Прямые:

**Хештеги:** #Programming/Databases/SQL

## Определение

**Производная таблица** — это виртуальная таблица, возвращаемая оператором `SELECT`. Производная таблица похожа на *временную таблицу*, но использовать производную таблицу в инструкции `SELECT` намного проще, чем временную таблицу, поскольку не требуется создавать временную таблицу.

Термины «производная таблица» и «подзапрос» часто используются как взаимозаменяемые. Когда автономный подзапрос используется в предложении FROM инструкции SELECT, его также называют производной таблицей.

Автономный подзапрос — это подзапрос, который может выполняться независимо от внешнего запроса. В отличие от подзапроса, производная таблица должна иметь псевдоним, чтобы можно было ссылаться на ее имя позже в запросе. Если производная таблица не имеет псевдонима, MySQL выдаст следующую ошибку: `Every derived table must have its own alias.`

Синтаксис подзапроса с производной таблицей выглядит так:

```sql
SELECT 
    select_list
FROM
    (SELECT 
        select_list
    FROM
        table_1) derived_table_name
WHERE 
    derived_table_name.c1 > 0;
```

## Пример

Следующий запрос получает пять продуктов с наибольшим доходом от продаж в 2003 году из таблиц «orders» и «`orderdetails`» в образце базы данных:

```sql
SELECT 
    productCode, 
    ROUND(SUM(quantityOrdered * priceEach)) sales
FROM
    orderdetails
        INNER JOIN
    orders USING (orderNumber)
WHERE
    YEAR(shippedDate) = 2003
GROUP BY productCode
ORDER BY sales DESC
LIMIT 5;
```

То же самое только с помощью результата этого запроса как производной таблицы, объединенной с таблицей продуктов следующим образом:

```sql
SELECT 
    productName, sales
FROM
    (SELECT 
        productCode, 
        ROUND(SUM(quantityOrdered * priceEach)) sales
    FROM
        orderdetails
    INNER JOIN orders USING (orderNumber)
    WHERE
        YEAR(shippedDate) = 2003
    GROUP BY productCode
    ORDER BY sales DESC
    LIMIT 5) top5products2003
INNER JOIN
    products USING (productCode);

```

В этом примере: 
1) Сначала выполняется подзапрос для создания набора результатов или производной таблицы.
2) Затем выполняется внешний запрос, который объединяет производную таблицу `top5product2003` с таблицей продуктов с использованием столбца `ProductCode`.