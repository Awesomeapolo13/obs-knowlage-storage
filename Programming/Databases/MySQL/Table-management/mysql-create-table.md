
# Создать таблицу

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Работа с таблицами](mysql-table-management)
- Прямые:
	- [Механизмы хранения](mysql-storage-engine);

**Хештеги:** #Programming/Databases/MySQL/Tables

## Синтаксис

Оператор `CREATE TABLE` позволяет создать новую таблицу в базе данных.

```sql
CREATE TABLE [IF NOT EXISTS] table_name(
   column1 datatype constraints,
   column2 datatype constraints,
) ENGINE=storage_engine;
```

Здесь:
- `table_name` - имя создаваемой таблицы;
- `column1`, `column2` - названия колонок таблицы;
- `datatype` - тип данных каждого столбца, такие как `INT`, `VARCHAR`, `DATE` и т. д.;
- `constraints` - необязательные ограничения, такие как `NOT NULL`, `UNIQUE`, `PRIMARY KEY` и `FOREIGN KEY`.

Если создать таблицу с именем, которое уже существует в БД, произойдет ошибка. Чтобы избежать ошибки, используется опция `IF NOT EXISTS`.

В MySQL каждая таблица имеет механизм хранения, такой как `InnoDB` или `MyISAM`. Выражение `ENGINE` позволяет указать механизм хранения таблицы.
