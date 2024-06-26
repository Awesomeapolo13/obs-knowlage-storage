
# Упорядоченные множества

**Ключи:**
- Обратные:
	- [Redis](redis);
	- [Типы данных](redis-data-types)
- Прямые:

**Хештеги:** 
- #Programming/Databases/Redis/DataTypes
- #Redis/DataTypes

## Особенности

1) Содержат только уникальные значения.
2) Для каждого значения указывается счет (score).
3) Значения автоматически сортируются по счету.
4) Время добавления и поиска: `O(log n)`.

![[Pasted image 20240401174550.png]]

## Сценарии использования

- рейтинги;
- аналог индекса в РСУБД (значение - ключ сущности).

*Важно помнить:*
- для вывода в порядке убывания используется zrev*;
- если обновить рейтинг, он перезапишет старый.

## Команды

1) Добавить элемент (при добавлении дубликата множество не изменится).
```redis
ZADD users:rating 10 user:1
```

2) Получить мощность множества.
```redis
ZCARD users:rating
```

3) Получить количество элементов в диапазоне.
```redis
ZCOUNT users:rating 5 10
```

4) Получить элементы в диапазоне.
```redis
ZRANGE users:rating 5 10 BYSCORE
```

5) Получить место элемента в множестве.
```redis
ZRANK users:rating user:12
```

6) Удалить элемент из множества.
```redis
ZREM users:rating user:12
```
