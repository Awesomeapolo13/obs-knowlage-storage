
# Очищение таблицы TRUNCATE TABLE

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Работа с таблицами](mysql-table-management)
- Прямые:
	- [Механизмы хранения](mysql-storage-engine);
	- [DELETE](mysql-delete);

**Хештеги:** #Programming/Databases/MySQL/Tables

## Синтаксис

Выражение `TRUNCATE TABLE` позволяет очистить все строки из таблицы.

```sql
TRUNCATE [TABLE] table_name;
```

Ключевыое слово `TABLE` является опциональным. Однако, считается хорошим тоном использовать это ключевое слово, чтобы отличать выражение `TRUNCATE TABLE` от функции `TRUNCATE()`.

## Особенности

Если существуют какие-либо ограничения `FOREIGN KEY` из других таблиц, ссылающихся на очищаемую таблицу, выражение `TRUNCATE TABLE` завершится ошибкой.

Поскольку операция очищения вызывает неявную фиксацию, ее невозможно откатить. Кроме того, выражение `TRUNCATE TABLE` не запускает триггеры `DELETE`, связанные с очищаемой таблицей.

Выражение `TRUNCATE TABLE` сбрасывает значение в колонке `AUTO_INCREMENT` к его исходному значению (если таблица имеет колонку с автоинкрементом).

В отличие от `DELETE` число колонок на которые влияет `TRUNCATE TABLE` рано 0, что интерпретируется как отсутствие информации.

В целом `TRUNCATE TABLE` аналогичен `DELETE` без условной конструкции `WHERE`, которая удаляет все строки из таблицы, или как последовательность операций `DROP TABLE` и `CREATE TABLE`:

```sql
DELETE FROM table_name
```

Однако, `TRUNCATE TABLE` более эффективное чем `DELETE`, потому что оно удаляет и пересоздает таблицу вмето удаления каждой страки одна за одной.
