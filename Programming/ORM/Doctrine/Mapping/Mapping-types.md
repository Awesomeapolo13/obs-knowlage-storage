Каждый PHP объект, который вы ходите сохранить в БД с помощью Doctrine называется Сузностью (Entity). Термин Сущности описывает объекты, которые имеют идентификатор (идентичность) среди многих независимых запросов (объекты с разными id или уникальными первичными ключами, которые подчеркивают различия между этими объектами). Данная идентичность определяется уникальным идентификатором этой сущности.

Doctrine - внешняя библиотека, поэтому она знает лишь о тех сущностях, чья структура будет описана с помощью механизма сопоставления метаданных (mapping metadata), которые сообщает библиотеке как сущность следует хранить в БД.

Doctrine поддерживает следующие типы маппинга:
- Аттрибутами;
- Аннотациями;
- XML;
- PHP;
- YAML (устарел и не используется в doctrine/orm 3.0).

**Ключи:**

- [XML-mapping](XML-mapping.md);

**Хештеги:**
- #Programming/ORM/Doctrine/Mapping