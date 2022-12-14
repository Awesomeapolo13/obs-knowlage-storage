
# Модуль (Module)

Модульный подход помогает структурно разграничить функциональность в большом приложении. Код, организованный по модульному принципу, обеспечивает низкую связанность между разными функциональными частями системы, позволяя вести разработку в ограниченном контексте, сосредоточив все свое внимание на нем.

При разделении приложения на модули следует опираться на принцип: **один модуль = один ограниченный контекст**.

На практике часта встречается подход с добавлением предохранительнго слоя (ACL), когда один модуль создает внешний интерфейс API, а другой добавляет его реализацию. Такой подхлд прекрасно ложится на монолитную и микросервисную архитектуру.

**Ключи:**
- [Список структурных элементов](Structural-element);
- [Тактическое проектирование](DDD-tactical-design);

**Хештеги:** #Programming/Arch/Structures