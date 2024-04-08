
# Транзакции

**Ключи:**
- Обратные:
	- [Redis](redis);
- Прямые:

**Хештеги:** 
- #Programming/Databases/Redis
- #Redis

## Решаемая проблема

При работе с Redis часто нужно отпралять наборы команд, при этом часто возникает существуенный сетевой overhead, что заметно влияет на производительность (потому что соединение после каждой команды обрывается и устанавливается заново).

## Решения

### Пайплайн (Pipeline)

- реализуется на клиенте (это не отдельная каманда Redis);
- клиент в одном сообщении просто отправляет несколько команд, разделенных разрывом строки;
- Redis возвращает сразу несколько ответов.

*Плюсы:*
- тривиальная реализация на клиенте.

*Минусы:*
- ответ, в том числе сообщения об ошибказ, вернётся только после попытки полного выполнения всех команд;
- Redis не гарантирует атомарность выполнения команд внутри пайплайна (т.е. если запустить одновременно два пайплайна но их команды перемешаются между собой).

### Транзакция

Позволяет выполнить несколько команд как одну, так же имеет механизм отката.
Команды после начала транзакции ставятся в очерердь. После завершения транзакции другие клиенты ставятся на паузу, и выполняются команды из очереди.

```redis
MULTI        # начало транзакции

SET t1 "a"   # Данные команды не выполняются,
SET t2 10    # а ставятся в очередь
INCR t2      #

EXEC         # Фиксация транзакции, все команды выше выполняются
DISCARD      # Откат транзакции
```

*Обработка ошибок*
- Если возникнет синтаксическая ошибка - транзакция сразу откатывается.
- Если возникнет логическая ошибка - команды транзакции продолжат выполняться, что может привести систему в неконсистентное состояние.

Для обработки логических ошибок необходимо использовать `DISCARD`.

Так же Redis позволяет имитировать "изоляцию" транзакций:
- все команды выполняются только в момент фиксации транзакции;
- есть риск, что за это время другой клиент изменит значения нужных нам ключей;
- Redis позволяет отслеживать конкретные ключи;
- если их значение изменит кто-то другой - транзакция просто не выполнится.

```redis
WATCH w1     # начало отслеживания значения w1

SET t1 10    # Начиная с этого момента (даже до MULTI) и вплоть до EXEC
             # любое реальное изменение w1 приведет к откату транзакции.
MULTI
SET t2 10
EXEC
```

*Плюсы:*
- гарантируют атомарность.

*Минусы:*
- выполняются только в момент вызова EXEC;
- есть нюансы, связанные с обработкой ошибок.