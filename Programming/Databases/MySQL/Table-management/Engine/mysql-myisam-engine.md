
# MyISAM

**Ключи:**
- Обратные:
	- [Работа с таблицами](mysql-table-management);
	- [Механизмы хранения](mysql-storage-engine);
- Прямые:

**Хештеги:** #Programming/Databases/MySQL/Tables/Engine


## Определение

MyISAM расширяет прежнюю систему хранения ISAM. Таблицы MyISAM оптимизированы по сжатию и скорости.

Таблицы MyISAM также переносимы между платформами и операционными системами. Размер таблицы MyISAM может достигать 256 ТБ, что очень много. Кроме того, таблицы MyISAM можно сжимать в таблицы, доступные только для чтения, для экономии места. При запуске MySQL проверяет таблицы MyISAM на предмет повреждений и даже восстанавливает их в случае ошибок. Недостаток таблиц MyISAM заключается в том, что они небезопасны для транзакций. До версии 5.5 MySQL использовал MyISAM в качестве механизма хранения по умолчанию.

Начиная с версии 5.5, MySQL использует InnoDB в качестве механизма хранения по умолчанию.