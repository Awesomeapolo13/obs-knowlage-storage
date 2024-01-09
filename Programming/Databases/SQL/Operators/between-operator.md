
# Оператор `BETWEEN`

**Ключи:**
- Обратные:
	- [SQL](SQL);
	- [Реляционные БД](relative);
- Прямые:

**Хештеги:** #Programming/Databases/SQL/Operator

## Описание

Оператор `BETWEEN` — это логический оператор, который определяет, находится ли значение в диапазоне.

```sql
value BETWEEN low AND high;
```

Оператор `BETWEEN` вернет 1 если:

```sql
value >= low AND value <= high
```

Если значение, `low` или `high`, равно `NULL`, оператор `BETWEEN` возвращает `NULL`.

## Оператор `NOT BETWEEN`

Оператор `NOT BETWEEN` возвоащает `TRUE`, если значение не находится в указанном диапазоне.

```sql
value NOT BETWEEN low AND high
```

Оператор `NOT BETWEEN` возвращает 1, если:

```sql
value < low OR value > high
```

При наличии значения равного `NULL` в левой части оператора или в списке значений, поведение аналогично оператру `IN`.

## Использование с датами

Чтобы проверить, находится ли значение в диапазоне дат, следует явно привести (`CAST`) значение к типу `DATE`.

```sql
SELECT 
   orderNumber,
   requiredDate,
   status
FROM 
   orders
WHERE 
   requireddate BETWEEN 
     CAST('2003-01-01' AS DATE) AND 
     CAST('2003-01-31' AS DATE);
```