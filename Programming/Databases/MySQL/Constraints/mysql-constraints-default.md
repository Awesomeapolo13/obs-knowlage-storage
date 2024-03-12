
# DEFAULT

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Ограничения](mysql-constraints);
- Прямые:

**Хештеги:** #Programming/Databases/MySQL/Constraints 

## Определение

**Ограничение DEFAULT** — это ограничение, позволяющее установить специальное значение по умолчанию для колонки.
Значение по умолчанию должно быть константой, например числом или строкой. Это не может быть функция или выражение. Однако MySQL позволяет устанавливать текущую дату и время (`CURRENT_TIMESTAMP`) в столбцы типа `TIMESTAMP` и `DATETIME`.

Если при операциях `INSERT` или `UPDATE` будет упущено значение колонки, то MySQL подставит в эту колонку значение по умолчанию, установленное ограничением `DEFAULT`.

## Синтаксис

Базовый синтаксис для ограничения `DEFAULT`:

```sql
column_name data_type DEFAULT default_value;
```

Чтобы добавить ограничение `DEFAULT` для колонки:

```sql
ALTER TABLE table_name
ALTER COLUMN column_name SET DEFAULT default_value;
```

Чтобы удалить ограничение `DEFAULT` для колонки:

```sql
ALTER TABLE table_name
ALTER column_name DROP DEFAULT;
```
