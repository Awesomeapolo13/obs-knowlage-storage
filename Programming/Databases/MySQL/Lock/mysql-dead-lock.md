
# Deadlock в MySQL

**Ключи:**
- Обратные:
	- [MySQL](MySQL);
	- [Deadlock](dead-lock);
	- [Блокировки таблиц](mysql-table-lock)

**Хештеги:** 
- #Programming/Databases/MySQL/Lock
- #Programming/Databases/Lock

## Обнаружение дедлоков

1) Проверка заблокированных транзакций осуществляется командой ниже (информация о состоянии блокировок).

```sql
SHOW ENGINE INNODB STATUS\G
```

В выводе этой команды нужно найти секцию, связанную с дедлоками. Она покажет информацию о транзакциях, участвующих в дедлоке.

2) Отмена заблокированных транзакций

```sql
KILL <thread_id>;
```

Заменяем `<thread_id>` на идентификатор потока, который нужно завершить. Идентификатор можно найти в выводе команды `SHOW ENGINE INNODB STATUS` или с помощью команды `SHOW PROCESSLIST`:

```sql
SHOW PROCESSLIST;
```
