
# Отфильтрованные индексы


Некоторые движки (SQL Server или PostgreSQL), позволяют вам установить специальное условие для индекса. Это позволяет уменьшить количество записей (строк), которые будут проиндексированы, делая индекс меньше и, соответсвенно, быстрее.

Синтаксис:

```sql
CREATE INDEX IndexName ON TableName (IndexedColumn [, IndexedColumnN]) WHERE Condition;
```

Пример:

```sql
CREATE INDEX IX_Rental_PlannedReturn_fEffectiveReturnIsNULL ON Rental (PlannedReturn) WHERE EffectiveReturn IS NULL;
```

Код выше создаст индекс в таблице `Rebtal`. Он базируется на колонке `PlannedReturn`, но только для тех строк, где `EffectiveReturn` равен `NULL`.

Примечание: [Кластеризованные индексы](db-index-types-clustered) нельзя фильтровать, поскольку они должны хранить все данные в таблице.

**Ключи:**
- [Индексы](db-index);
- [Типы индексов](db-index-types);

**Хештеги:** #Programming/Databases/Index/Types