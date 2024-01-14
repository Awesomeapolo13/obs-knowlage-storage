
# Декораторы в CQRS

**Ключи:**
- Обратные:
	-  [CQRS](CQRS);

**Хештеги:** #Programming/Arch/CQRS

## Цели

Использование паттерна декоратора в контексте CQRS (Command Query Responsibility Segregation) может быть очень полезным для добавления дополнительного поведения или логики обработки без изменения основной функциональности команд (Commands) и запросов (Queries). Декоратор оборачивает объект и расширяет его функциональность, сохраняя при этом его интерфейс.

## Примеры ситуаций

1. **Логирование:** Декоратор может автоматически логировать детали выполнения команды или запроса, не требуя изменений в основной бизнес-логике.
    
2. **Валидация:** Декоратор может использоваться для проверки входных данных команды или запроса перед их выполнением.
    
3. **Транзакционность:** В случае команд, декоратор может обеспечить выполнение операций в рамках транзакции.
    
4. **Кэширование:** Для запросов декоратор может добавить кэширование результатов, чтобы уменьшить нагрузку на базу данных.

## Пример на PHP

Допустим, у нас есть простая система CQRS, и мы хотим добавить логирование для команд.

**Базовый Обработчик Команд**

```php
interface CommandHandlerInterface {
    public function handle($command);
}

class CreateUserCommandHandler implements CommandHandlerInterface {
    public function handle($command) {
        // Логика создания пользователя
    }
}
```

Декоратор для логирования:

```php
class LoggingCommandHandlerDecorator implements CommandHandlerInterface {
    private $wrappedHandler;

    public function __construct(CommandHandlerInterface $handler) {
        $this->wrappedHandler = $handler;
    }

    public function handle($command) {
        // Логирование перед выполнением команды
        echo "Executing command: " . get_class($command) . "\n";

        // Выполнение команды
        $this->wrappedHandler->handle($command);

        // Логирование после выполн
```

Реализация:
```php

#### Использование Декоратора

```php
// Создание обработчика команды
$createUserHandler = new CreateUserCommandHandler();

// Оборачивание обработчика команды в декоратор для логирования
$loggedCreateUserHandler = new LoggingCommandHandlerDecorator($createUserHandler);

// Создание команды
$createUserCommand = new CreateUserCommand(/* параметры команды */);

// Выполнение команды с логированием
$loggedCreateUserHandler->handle($createUserCommand);
```

В этом примере `LoggingCommandHandlerDecorator` декорирует `CreateUserCommandHandler`. Когда команда передается декоратору, он сначала выполняет логирование, затем делегирует выполнение команды обернутому обработчику и снова логирует после выполнения. Это позволяет добавить логирование к процессу обработки команды без изменения основной логики `CreateUserCommandHandler`.

Таким образом, декораторы в CQRS предоставляют гибкий способ расширения функциональности обработчиков команд и запросов, сохраняя при этом их основную ответственность и интерфейс.