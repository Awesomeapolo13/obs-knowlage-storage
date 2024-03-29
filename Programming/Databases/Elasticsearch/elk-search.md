
# Elasticsearch

**Ключи:**
- Обратные:
	- [Базы данных](databases);
	- [NoSQL   БД](doc-oriented);
- Прямые:
	- [Высокоуровневая архитектура](elk-high-load-arch);
	- [Индекс](elk-index);
	- [Документ](elk-doc);
	- Шарды и реплики;
	- Тип данных;
	- [Маппинг](elk-mapping);
	- Анализаторы и токенизаторы;
	- [Инвертированный индекс](elk-inverted-index);
	- Операции;
	- Поиск;

**Хештеги:** 
- #Programming/Databases/NoSQL
- #Programming/Databases/Elasticsearch
- #ElasticStack

## Описание

Elasticsearch — это мощная открытая поисковая и аналитическая система для всех типов данных, включая текстовые, числовые, геопространственные, структурированные и неструктурированные, основанная на Apache Lucene. Она предоставляет распределенное хранилище данных, поддержку полнотекстового поиска и аналитики в реальном времени для больших объемов данных. Основное использование - полнотекстовый поиск.
## Особенности

- документо-ориентированная БД;
- стек - Java + Apache Lucene;
- взаимодействие через REST API + JSON;
- гибкая конфигурация типов данных (от автоматического ввода, до строгих ограничений по типам);
- огромные возможности поиска, в т.ч. полнотекстого (даже на русском).

## Для чего использовать

- сбор, хранение и анализ логово;
- работа с метриками, в т.ч. бизнес-метриками;
- полнотекстовый поиск по документам;
- работа с картами и гео-данными.

## Нюансы эксплуатации

- повышенное потребление ресурсов;
- требуются быстрые и ёмкие диски;
- не гарантируется согласованность данных;
- не рекомендуется использование в качестве основной БД.

## Ключевые концепции

1) **Индекс** - база данных.
2) **Документ** - запись в индексе.
3) **Маппиниг** - типы данных для конкретных полей.
