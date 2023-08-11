
# Функциональный индекс (выражение)

Некоторые механизмы баз данных, такие как Oracle, DB2, Informix или PostgreSQL, позволяют пользователям создавать индексы для детерминированных выражений, включая предоставляемые системой или определяемые пользователем функции, без необходимости создавать расчитываемый или вычисляемый столбец в таблице (SQL Server требует создания вычисляемого столбца).

Синтаксис:

```sql
CREATE INDEX IndexName
ON TableName (Expression);
```

Пример:

```sql
CREATE INDEX IX_Rental_RentalYear
ON Rental (EXTRACT(YEAR FROM RentalDate));
```

С помощью этого индекса следующий запрос будет использовать индекс для поиска всех продуктов, в которых символы в позициях с 3 по 6 ProductCode являются «TECH»:

```sql
SELECT LicensePlate, Brand, Model
FROM Vehicle
WHERE LicensePlate = ‘6TRJ999’;
```


**Ключи:**
- [Индексы](db-index);
- [Типы индексов](db-index-types);

**Хештеги:** #Programming/Databases/Index/Types