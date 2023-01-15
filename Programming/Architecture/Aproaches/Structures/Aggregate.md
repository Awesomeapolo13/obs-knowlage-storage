
# Аггрегат (Aggregate)

Аггрегат - концептуальное понятие, включающее корневую сущность, и другие принадлежащие ей сущности, объекты значения.

Такая композиция способна обеспечить согласованность транзакций в пределах границы агрегата. В случае изменения состояния агрегата, должны быть обеспечены бизнес-инварианты.

То есть при сохранении аггрегата должны сохраняться, обновляться или удаляться все связанные с ним сущности без каких либо задержек.

**Ключи:**
- [Список структурных элементов](Structural-element);
- [Тактическое проектирование](DDD-tactical-design);

**Хештеги:** #Programming/Arch/Structures