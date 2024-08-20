
# UPDATE JOIN

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Модификация данных MySQL](mysql-modifying-data);
	- [UPDATE](mysql-update);

**Хештеги:** 
- #Programming/Databases/MySQL/Modify

## Синтаксис

### Базовый

В MySQL так же возможно использовать оператор `JOIN` вместе с оператором `UPDATE`, чтобы изменить данные в одной таблице, на основе данных из другой таблицы. Так же `UPDATE JOIN` полезен при изменении данных в нескольких таблицах:

```sql
UPDATE T1
[INNER JOIN | LEFT JOIN] T2 ON T1.C1 = T2.C1
SET T1.C2 = T2.C2, 
    T2.C3 = expr
WHERE condition;
```

Без использования `UPDATE JOIN` так же можно обновить данные нескольких таблиц. Выражение ниже эквивалентно использованию `INNER JOIN`:

```sql
UPDATE T1
SET T1.c2 = T2.c2,
    T2.c3 = expr
WHERE T1.c1 = T2.c1 AND condition;
```

Эквивалентное выражение с `UPDATE JOIN`:

```sql
UPDATE T1
INNER JOIN T2 ON T1.C1 = T2.C1
SET T1.C2 = T2.C2,
      T2.C3 = expr
WHERE condition;
```
