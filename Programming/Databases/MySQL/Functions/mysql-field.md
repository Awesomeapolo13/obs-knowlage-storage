
# FIELD

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [SQL](SQL);
	- [Функции MySQL](mysql-functions);

**Хештеги:** #Programming/Databases/MySQL/Func

## Определение

Функция `FIELD()` возвращает индекс (позицию) значения в списке значений.

```sql
FIELD(value, value1, value2, ...)
```

- `value` - значение, по которому вы хотите найти позицию;
- `value1, value2, ...` - список значений, с которыми вы хотите сравнить указанное значение.

## Использование

Функция `FIELD()` возвращает позицию значения в списке значений `value1, value2` и т. д. Если значение не найдено в списке, функция `FIELD()` возвращает `0`.

Выражение ниже вернет 1, потому что строка `A` первая в списке `A, B, C`:

```sql
SELECT FIELD('A', 'A', 'B','C');
```
