
# Персистентность

**Ключи:**
- Обратные:
	- [Redis](redis);
- Прямые:

**Хештеги:** 
- #Programming/Databases/Redis
- #Redis

## Определение

Сохранение данных из ОЗУ на постоянное хранилище.

## Режимы

### RDB (Redis Database)

В Redis можно реализовать с помощью RDB (Redis Database).

Сохраняет полные слепки (snapshots) данных:

- вся БД сохраняется полностью в один файл;
- идеально для бэкапов с быстрым разворачиванием;
- обычно создается раз в S секунд или после N операций записи.

Используется, когда допустимо потерять часть данных (аналитика).

### AOF(Append Only File)

Просто добавляет в файлик все данные покомандно (пришла команда - записал в файл).

Инкрементный лог изменений.
- аналог WAL;
- минимальная (или нулевая) потеря свежих данных;
- более устойчивый к потере питания;
- в целом медленнее, чем RDB.

Используется, когда важна целостность данных (event sourcing, streams и др.).
Файл не будет разрастаться бесконечно, спустя какое то время, Redis сделать слепок и переведет все данные в RDB.