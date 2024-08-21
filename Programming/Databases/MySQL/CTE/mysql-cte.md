
# CTE (Common Table Expression)

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
- Прямые:
	- [Recursive CTE](mysql-cte-recursive);

**Хештеги:** #Programming/Databases/MySQL/CTE 


## Определение

Общее табличное выражение (Common Table Expression) — это именованный временный набор результатов, который существует исключительно в области выполнения одного оператора SQL, например `SELECT`, `INSERT`, `UPDATE` или `DELETE`.

Подобно производной таблице (таблица формируемая в памяти после выполнения оператора `SELECT`), общее табличное выражение (`CTE`) не сохраняется как объект и действует только во время выполнения запроса.

В отличие от производной таблицы, общее табличное выражение (`CTE`) может ссылаться на себя (в случае рекурсивного `CTE`) или на него можно ссылаться несколько раз в одном и том же запросе. Более того, `CTE` обеспечивает улучшенную читаемость и производительность по сравнению с производной таблицей.

## Синтаксис

Структура `CTE` включает имя, опциональный список колонок и запрос, который определяет `CTE`. После определения `CTE` его можно использовать как представление в операторах `SELECT`, `INSERT`, `UPDATE`, `DELETE` или `CREATE VIEW`.

```sql
WITH cte_name (column_list) AS (
    query
) 
SELECT * FROM cte_name;
```

- `WITH cte_name (column_list) AS` определяет `CTE` с именем `cte_name` и списком столбцов (`column_list`), которые будут иметь `CTE`. `Column_list` является необязательным, если его не указать, `CTE` унаследует имена столбцов из результата запроса;
- `query` - это запрос, который определеяет `CTE`. MySQL будет хранить результат этого запроса в `CTE`;
- `SELECT * FROM cte_name` - это пример запроса, который может использовать `CTE` (результаты запроса `CTE`). В данном случае он просто выводит все результаты из `CTE`.

## Примеры

### Базовый

Определяем `CTE` с именем customers_in_usa (хранит имя покупателя и его штат в США). Затем определяем запрос, который получает эти данные из таблицы `customers`. Далее в запросе после `CTE` выбираем из `CTE` покупателей из Калифорнии.

```sql
WITH customers_in_usa AS (
    SELECT 
        customerName, state
    FROM
        customers
    WHERE
        country = 'USA'
) SELECT 
    customerName
 FROM
    customers_in_usa
 WHERE
    state = 'CA'
 ORDER BY customerName;
```

Так же внутри `CTE` можно использовать `JOIN` и другие выражения. Можно так же делать `JOIN` с результатами `CTE`. Например, в следующем примере `CTE` используется для получения пяти крупнейших торговых представителей на основе их общего объема продаж в 2003 году:

```sql
WITH topsales2003 AS (
    SELECT 
        salesRepEmployeeNumber employeeNumber,
        SUM(quantityOrdered * priceEach) sales
    FROM
        orders
            INNER JOIN
        orderdetails USING (orderNumber)
            INNER JOIN
        customers USING (customerNumber)
    WHERE
        YEAR(shippedDate) = 2003
            AND status = 'Shipped'
    GROUP BY salesRepEmployeeNumber
    ORDER BY sales DESC
    LIMIT 5
)
SELECT 
    employeeNumber, 
    firstName, 
    lastName, 
    sales
FROM
    employees
        JOIN
    topsales2003 USING (employeeNumber);
```

Сначала определяем топ 5 сотрудников по их продажам в 2003 году. Затем присоединяем таблицу `CTE` к таблице сотрудников, чтобы включить именя и фамилии торговых пердставителей.

### Использование множества `CTE`

![[Pasted image 20240821103701.png]]

В следующем примере используется несколько `CTE` для сопоставления клиентов с их соответствующими торговыми представителями:

```sql
WITH salesrep AS (
    SELECT 
        employeeNumber,
        CONCAT(firstName, ' ', lastName) AS salesrepName
    FROM
        employees
    WHERE
        jobTitle = 'Sales Rep'
),
customer_salesrep AS (
    SELECT 
        customerName, salesrepName
    FROM
        customers
            INNER JOIN
        salesrep ON employeeNumber = salesrepEmployeeNumber
)
SELECT 
    *
FROM
    customer_salesrep
ORDER BY customerName;
```

- `CTE salesrep`: выбераем `employeeNumber` и объедините столбцы `frstName` и `lastName`, чтобы создать столбец с именем `salesRepName` и включить только сотрудников с должностью «Торговый представитель»;
- `CTE` `customer_salesrep`: выбирает `customerName` и `salesrepName`, соединяя таблицу `customers` с `CTE` `salesrep` на основе общего столбца `employeeNumber`;
- Основной запрос: выберает все столбцы из `CTE` `customer_salesrep`.

### Слияние двух `CTE`

![[Pasted image 20240821104238.png]]

В следующем примере создаются два `CTE` и соединяется, чтобы получить информацию о торговых представителях, находящихся в США, включая информацию об их офисах:

```sql
WITH e AS (
  SELECT 
    * 
  FROM 
    employees 
  WHERE 
    jobTitle = 'Sales Rep'
), 
o AS (
  SELECT 
    * 
  FROM 
    offices 
  WHERE 
    country = 'USA'
) 
SELECT 
  firstName, 
  lastName, 
  city, 
  state, 
  postalCode 
FROM 
  e 
  INNER JOIN o USING (officeCode);
```

- `CTE e`: получить сотрудников, чья должность — `Sales Rep`.
- `CTE o`: Офисы поиска, расположенные в США.
- Главный запрос: Объединяет `CTE` `e` и `o` с помощью столбца `officeCode`.
