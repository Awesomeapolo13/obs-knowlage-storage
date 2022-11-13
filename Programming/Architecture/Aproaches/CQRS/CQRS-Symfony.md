
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

7) Теперь можно создать комманду, например, создание пользователя
Создадим в модуле, отвечающем за пользователя, на слое Приложения (Application/Command/CreateUser/CreateUserCommand.php), комманду и ее обработчик.

```php
<?php  
  
declare(strict_types=1);  
  
namespace App\Users\Application\Command\CreateUser;  
  
use App\Shared\Application\Command\CommandInterface;  
  
class CreateUserCommand implements CommandInterface  
{  
    public function __construct(  
        public readonly string $email,  
        public readonly string $password  
    ) {  
    }
}

```

Поскольку это ДТО, то свойства публичные.

```php
<?php  
  
declare(strict_types=1);  
  
namespace App\Users\Application\Command\CreateUser;  
  
use App\Shared\Application\Command\CommandHandlerInterface;  
use App\Users\Domain\Factory\UserFactory;  
use App\Users\Domain\Repository\UserRepositoryInterface;  
  
class CreateUserCommandHandler implements CommandHandlerInterface  
{  
    public function __construct(private readonly UserRepositoryInterface $userRepository)  
    {  
    }  
    /**  
     * Создает пользователя и возвращает его идентификатор     */    public function __invoke(CreateUserCommand $createUserCommand): string  
    {  
        $user = (new UserFactory())->create($createUserCommand->email, $createUserCommand->password);  
        $this->userRepository->add($user);  
  
        return $user->getUlid();  
    }  
}

```

8) Напишем функциональный тест для этой комманды

```php
<?php  
  
namespace App\Tests\Functional\Users\Application\Command\CreateUser;  
  
use App\Shared\Application\Command\CommandBusInterface;  
use App\Tests\Tools\FakerTools;  
use App\Users\Application\Command\CreateUser\CreateUserCommand;  
use App\Users\Domain\Repository\UserRepositoryInterface;  
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;  
  
class CreateUserCommandHandler extends WebTestCase  
{  
    use FakerTools;  
  
    private CommandBusInterface $commandBus;  
    private UserRepositoryInterface $userRepository;  
  
    public function setUp(): void  
    {  
        parent::setUp();  
        $this->commandBus = $this::getContainer()->get(CommandBusInterface::class);  
        $this->userRepository = $this::getContainer()->get(UserRepositoryInterface::class);  
    }  
  
    public function test_user_created_successfully(): void  
    {  
        $command = new CreateUserCommand($this->getFaker()->email(), $this->getFaker()->password());  
  
        // act  
        $userUlid = $this->commandBus->execute($command);  
  
        // assert  
        $user = $this->userRepository->findByUlid($userUlid);  
        $this->assertNotEmpty($user);  
    }  
}

```

9) Создадим запрос на получение пользователя по email.
Это ДТО, принимающее значение email.

```php
<?php  
  
declare(strict_types=1);  
  
namespace App\Users\Application\Query\FindUserByEmail;  
  
use App\Shared\Application\Query\QueryHandlerInterface;  
use App\Users\Application\DTO\UserDTO;  
use App\Users\Domain\Repository\UserRepositoryInterface;  
  
class FindUserByEmailQueryHandler implements QueryHandlerInterface  
{  
    public function __construct(  
        private readonly UserRepositoryInterface $userRepository  
    ) {  
    }  
    public function __invoke(FindUserByEmailQuery $query): ?UserDTO  
    {  
        $user = $this  
            ->userRepository  
            ->findByEmail($query->email);  
  
        if (!$user) {  
            return null;  
        }  
  
        return UserDTO::fromEntity($user);  
    }  
}
```

10) Создадим DTO на прикладном уровне. Создание производится статичным методом из сущности.

```php
<?php  
  
declare(strict_types=1);  
  
namespace App\Users\Application\DTO;  
  
use App\Users\Domain\Entity\User;  
  
class UserDTO  
{  
    public function __construct(  
        public readonly string $ulid,  
        public readonly string $email  
    ){  
    }  
    public static function fromEntity(User $user): self  
    {  
        return new self($user->getUlid(), $user->getEmail());  
    }  
}
```

11) Создаlbv тест для проверки работы запроса полиска пользователя по электронной почте

```php
<?php  
  
namespace App\Tests\Functional\Users\Application\Query\FindUserByEmail;  
  
use App\Shared\Application\Query\QueryBusInterface;  
use App\Tests\Resource\Fixture\UserFixture;  
use App\Users\Application\DTO\UserDTO;  
use App\Users\Application\Query\FindUserByEmail\FindUserByEmailQuery;  
use App\Users\Domain\Entity\User;  
use App\Users\Domain\Repository\UserRepositoryInterface;  
use Faker\Factory;  
use Liip\TestFixturesBundle\Services\DatabaseToolCollection;  
use Symfony\Bundle\FrameworkBundle\Test\WebTestCase;  
  
class FindUserByEmailQueryHandlerTest extends WebTestCase  
{  
    public function setUp(): void  
    {  
        parent::setUp();  
        $this->faker = Factory::create();  
        $this->queryBus = $this::getContainer()->get(QueryBusInterface::class);  
        $this->userRepository = $this::getContainer()->get(UserRepositoryInterface::class);  
        $this->databaseTool = static::getContainer()->get(DatabaseToolCollection::class)->get();  
    }  
  
    public function test_user_created_when_command_executed(): void  
    {  
        // arrange  
        $referenceRepository = $this->databaseTool->loadFixtures([UserFixture::class])->getReferenceRepository();  
        /** @var User $user */  
        $user = $referenceRepository->getReference(UserFixture::REFERENCE);  
        $query = new FindUserByEmailQuery($user->getEmail());  
  
        // act  
        $userDTO = $this->queryBus->execute($query);  
  
        // assert  
        $this->assertInstanceOf(UserDTO::class, $userDTO);  
    }  
}
```

