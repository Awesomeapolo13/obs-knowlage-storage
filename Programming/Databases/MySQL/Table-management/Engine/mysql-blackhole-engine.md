
# BLACKHOLE

**Ключи:**
- Обратные:
	- [Работа с таблицами](mysql-table-management);
	- [Механизмы хранения](mysql-storage-engine);
- Прямые:

**Хештеги:** #Programming/Databases/MySQL/Tables/Engine


## Определение

Механизм хранения BLACKHOLE не хранит данные таблицы. Это означает, что данные, которые вы отправляете в таблицу BLACKHOLE, отбрасываются.

Таким образом, таблицы BLACKHOLE могут быть полезны в сценарии репликации, когда вы хотите фиксировать изменения данных на главном сервере, не сохраняя эти данные на локальном сервере.
