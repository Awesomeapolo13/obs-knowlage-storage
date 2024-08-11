
# Deadlock в Postgre

**Ключи:**
- Обратные:
	- [PostgreSQL](PostgreSQL);
	- [Deadlock](dead-lock)

**Хештеги:**
- #Programming/Databases/Postgre/Lock;
- #Programming/Databases/Lock;

## Обнаружение дедлоков

1) Проверка заблокированных процессов осуществляется командой ниже (информация о состоянии блокировок) через системное представление pg_lock.

```sql
SELECT 
    pg_stat_activity.pid,
    pg_stat_activity.query,
    pg_locks.locktype,
    pg_locks.mode,
    pg_locks.granted
FROM 
    pg_stat_activity
JOIN 
    pg_locks 
ON 
    pg_stat_activity.pid = pg_locks.pid
WHERE 
    pg_locks.granted = false;

```

Запрос выведет процессы, ожидающие блокировок.

2) Отмена заблокированных процессов через команжу `pg_terminate_backend`

```sql
SELECT pg_terminate_backend(pid)
FROM pg_stat_activity
WHERE pid = <process_id>;
```

Заменяем `<process_id>` на идентификатор процесса, который должен быть завершен.
