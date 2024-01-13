
# Изменение таблицы

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Работа с таблицами](mysql-table-management)
- Прямые:
	- [Механизмы хранения](mysql-storage-engine);

**Хештеги:** #Programming/Databases/MySQL/Tables

## Синтаксис

### Добавление одного столбца

Оператор `ALTER TABLE ADD` позволяет добавить в таблицу один или несколько столбцов.

Выражение ниже добавляет один столбец в таблицу:

```sql
ALTER TABLE table_name
ADD 
    new_column_name column_definition
    [FIRST | AFTER column_name]

```

Здесь:
- `table_name` – определяет имя таблицы, в которую нужно добавить новую колонку или колонки после ключевых слов `ALTER TABLE`;
- `new_column_name` –  определяет имя нового столбца;
- `column_definition`– определяет тип данных, максимальный размер и ограничение для нового столбца;
- `FIRST | AFTER column_name` - определяет позицию новой колонки в таблице, т.е. можно вставить столбец после уже существующего столбца (`ATER column_name`) или первым столбцом (`FIRST`) (если же опустить это выражение то столбец будет вставлен в конце таблицы).

### Добавление нескольких столбцов

Чтобы добавить в таблицу несколько столбцов, необходимо использовать следующую форму оператора `ALTER TALE ADD`:

```sql
ALTER TABLE table_name
    ADD new_column_name column_definition
    [FIRST | AFTER column_name],
    ADD new_column_name column_definition
    [FIRST | AFTER column_name],
    ...;

```

### Модификация  столбца

Хорошей практикой является просмотр атрибутов столбца перед его изменением. 

```sql
DESCRIBE table_name;
```

Далее, после просмотра и проверки аттрибутов можно возмользоваться базовым синтаксисом изменения столбца.

```sql
ALTER TABLE table_name
MODIFY column_name column_definition
[ FIRST | AFTER column_name];    
```

### Модификация нескольких столбцов

Основной синтаксис показан ниже. Изменение данных столбцов происходит последовательно начиная с первого столбца в выражении.

```sql
ALTER TABLE table_name
    MODIFY column_name column_definition
    [ FIRST | AFTER column_name],
    MODIFY column_name column_definition
    [ FIRST | AFTER column_name],
    ...;

```

### Переименование столбцов

Чтобы изменить имя столбца, воспользуйтесь следующим выражением:

```sql
ALTER TABLE table_name
    CHANGE COLUMN original_name new_name column_definition
    [FIRST | AFTER column_name];
```

Здесь:
- сначала, указывается имя таблицы, которой принадлежит столбец;
- затем, после оператора `CHANGE COlUMN` указывается старое и новое имя столбца, а так же определение его типа;
- опционально, можно указать после какого столбца разместить измененный.

### Удалить столбец

Удаление столюца осуществляется выражением `ALTER TABLE DROP COLUMN`:

```sql
ALTER TABLE table_name
DROP COLUMN column_name;

```

### Изменение имени таблицы

Для переименования таблицы необходимо использовать выражение `ALTER TABLE RENAME TO`:

```sql
ALTER TABLE table_name
RENAME TO new_table_name;
```
