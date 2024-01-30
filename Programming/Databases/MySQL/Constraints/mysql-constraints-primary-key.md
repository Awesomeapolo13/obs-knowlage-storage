
# Первичный ключ

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Ограничения](mysql-constraints);
- Прямые:

**Хештеги:** #Programming/Databases/MySQL/Constraints 

## Определение

В MySQL первичный ключ — это столбец или набор столбцов, который уникально идентифицирует каждую строку таблицы.

Столбец первичного ключа должен содержать уникальные значения. Если первичный ключ состоит из нескольких столбцов, комбинация значений в этих столбцах должна быть уникальной. Кроме того, столбец первичного ключа не может содержать NULL.

Таблица может иметь ноль или один первичный ключ, но не более одного.

## Синтаксис

Обычно первичный ключ для таблицы определяется при ее создании. Вот синтаксис определения первичного ключа, состоящего из одного столбца:

```sql
CREATE TABLE table_name(
   column1 datatype PRIMARY KEY,
   column2 datatype, 
   ...
);
```

В этом синтаксисе мы определяем ограничение `PRIMARY KEY` как ограничение столбца.

Кроме того, можно поместить `PRIMARY KEY` в конец списка столбцов:

```sql
CREATE TABLE table_name(
   column1 datatype,
   column2 datatype, 
   ...,
   PRIMARY KEY(column1)
);
```

В этом синтаксисе мы определяем ограничение `PRIMARY KEY` как ограничение таблицы.

Если первичный ключ состоит из двух или более столбцов, вам необходимо использовать ограничение таблицы для определения первичного ключа:

```sql
CREATE TABLE table_name(
   column1 datatype,
   column2 datatype,
   column3 datatype,
   ...,
   PRIMARY KEY(column1, column2)
);
```

Если существующая таблица не имеет первичного ключа, вы можете добавить в нее первичный ключ с помощью выражения `ALTER TABLE... ADD PRIMARY KEY`:

```sql
ALTER TABLE table_name
ADD PRIMARY KEY(column1, column2, ...);
```

На практике удаление первичного ключа довольно редко применяется. Однако если необходимо это сделать, можно использовать выражение `ALTER TABLE ... DROP PRIMARY KEY`:

```sql
ALTER TABLE table_name
DROP PRIMARY KEY;
```