
# Реализация CQRS для симфони

1) Определяем в общей директории интерфейсы для комманд и запросов

В слове приложения, создаем интерфейсы комманды, обработчика комманды и коммандной шины. То же самое делаем для запросов.

![[Pasted image 20221106204148.png]]

2) Создадим в интерфейсах коммандных шин методы execute с единственным аргументом в качестве интерфейса комманды и запроса соответсвенно

```php
interface CommandBusInterface  
{  
    public function execute(CommandInterface $command): mixed;  
}

interface QueryBusInterface  
{  
    public function execute(QueryInterface $query): mixed;  
}
```


3) Установим Symfony messager для работы с коммандной шиной и шиной запроса

```shell
composer require symfony/messenger
```

4) Определяем в файлике конфигураций messenger.yaml коммандную и запросную шины

```yaml
framework:  
    messenger:  
        default: command.bus  
        buses:  
            command.bus:  
                # Команды надо оборачивать в транзакции для обеспечения атомарности  
                middleware:  
                    - doctrine.transaction  
            query.bus:
```

5) Зарегистрируем хендлеры для коммандной и запросной шин в services.yml
Регистрировать будем с помощью интрефейсов.

```yaml
_instanceof:  
    App\Shared\Application\Command\CommandHandlerInterface:  
        tags:  
            - { name: messenger.message_handler, bus: command.bus }  
  
    App\Shared\Application\Query\QueryHandlerInterface:  
        tags:  
            - { name: messenger.message_handler, bus: query.bus }
```

6) Добавить реализацию коммандной шины на инфраструктурном слое общей папки

```php
<?php  
  
namespace App\Shared\Infrastructure\Bus;  
  
use App\Shared\Application\Command\CommandBusInterface;  
use App\Shared\Application\Command\CommandInterface;  
use Symfony\Component\Messenger\HandleTrait;  
use Symfony\Component\Messenger\MessageBusInterface;  
  
class CommandBus implements CommandBusInterface  
{  
    use HandleTrait;  
  
    public function __construct(MessageBusInterface $commandBus)  
    {  
        // имя переменной соответствует тому, что указано в конфиге  
        $this->messageBus = $commandBus;  
    }  
  
    public function execute(CommandInterface $command): mixed  
    {  
        return $this->handle($command);  
    }  
}

```

Аналогично реализуем и шину запросов.
