
# Реализация аутентификации по JWT в Symfony


Для реализации необходимы два бандла.
- [Бандл для работы с Access token](https://github.com/lexik/LexikJWTAuthenticationBundle)
- [Бандл для работы с Refresh token](https://github.com/markitosgv/JWTRefreshTokenBundle)

## Конфигурация бандлов

1) Установить бандлы
2) В файл конфигурации lexik_jwt_authentication.yaml внесем время жизни Access токена и поле идентификатора

```yaml
lexik_jwt_authentication:  
    secret_key: '%env(resolve:JWT_SECRET_KEY)%'  
    public_key: '%env(resolve:JWT_PUBLIC_KEY)%'  
    pass_phrase: '%env(JWT_PASSPHRASE)%'  
    token_ttl: 3600 # 1 hour  
    user_identity_field: email
```

3) В файле security.yaml укажем провайдера, в нашем случае app_entity_provider

```yaml
providers:  
    app_user_provider:  
        entity:  
            class: App\Users\Domain\Entity\User  
            property: email
```

4) Настроем фаервол. В разделе access_control пропишем пути для логина, обновление токена, получения информации о пользователе

```yaml
firewalls:  
    login:  
        pattern: ^/api/auth/token/login  
        stateless: true  
        json_login:  
            username_path: email  
            check_path: /api/auth/token/login  
            success_handler: lexik_jwt_authentication.handler.authentication_success  
            failure_handler: lexik_jwt_authentication.handler.authentication_failure  
  
    api:  
        pattern: ^/api  
        stateless: true  
        jwt: ~  
  
    access_control:  
        # Для логина  
        - { path: ^/api/auth/token/login, roles: PUBLIC_ACCESS }  
        # Для обновления токена  
        - { path: ^/api/auth/token/refresh, roles: PUBLIC_ACCESS }  
        # Для получения информации о пользователе  
        - { path: ^/api/users/me,       roles: IS_AUTHENTICATED_FULLY }
```

Для тестового окружения применим упрощенную стратегию формирования токена, а именно plaintext - текст без шибрования

5) Так же настроим конфиги для refresh bundle
Для этого в routes.yaml укажем:
- роут для обновления токена;
- роут для логина.

```yaml
gesdinet_jwt_refresh_token:  
    path: /api/auth/token/refresh  
  
api_login_check:  
    path: /api/auth/token/login
```

Добавим конфигурацию для рефреш токена в фаервол

```yaml
firewalls:
	...

	api_token_refresh:
		pattern: ^/api/auth/token/refresh
		stateless: true
		refresh_jwt: ~

	...
```

6) Создадим сущность для рефреш токена и настроим для нее маппинг

```php

<?php  
  
declare(strict_types=1);  
  
namespace App\Users\Domain\Entity;  
  
use Gesdinet\JWTRefreshTokenBundle\Entity\RefreshToken as BaseRefreshToken;  
  
/**
 * Класс рефреш токена 
 */
 class RefreshToken extends BaseRefreshToken  
{  
}

```

При выполнении маппинга нет необходимости прописывать поля. Достаточно создать xml файл с маппином и указать конфигурацию в doctrine.yml

```yaml

# doctrine.yml

doctrine:  
    orm:  
        auto_generate_proxy_classes: true  
        naming_strategy: doctrine.orm.naming_strategy.underscore_number_aware  
        auto_mapping: true  
        mappings:
          
            ..... 
  
            RefreshToken:  
                is_bundle: false  
                type: xml  
                dir: '%kernel.project_dir%/src/Users/Infrastructure/Database/ORM'  
                prefix: 'App\Users\Domain\Entity'  
                alias: RefreshToken

```

```xml
<doctrine-mapping  
        xmlns="http://doctrine-project.org/schemas/orm/doctrine-mapping"  
        xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"  
        xsi:schemaLocation="https://doctrine-project.org/schemas/orm/doctrine-mapping  
                   https://www.doctrine-project.org/schemas/orm/doctrine-mapping.xsd">  
  
    <entity name="App\Users\Domain\Entity\RefreshToken" table="refresh_token">  
  
    </entity>  
</doctrine-mapping>
```

7) Создадим и выполним миграцию для создания таблицы рефреш токенов

```shell
php bin/console doctrine:migrations:diff
```

```php

public function up(Schema $schema): void  
{  
    $this->addSql('CREATE SEQUENCE refresh_token_id_seq INCREMENT BY 1 MINVALUE 1 START 1');  
    $this->addSql(  
        'CREATE TABLE refresh_token (  
                        id INT NOT NULL,                        refresh_token VARCHAR(128) NOT NULL,                        username VARCHAR(255) NOT NULL,                        valid TIMESTAMP(0) WITHOUT TIME ZONE NOT NULL,                        PRIMARY KEY(id)                       )'  
    );  
    $this->addSql('CREATE UNIQUE INDEX UNIQ_C74F2195C74F2195 ON refresh_token (refresh_token)');  
    $this->addSql('CREATE UNIQUE INDEX UNIQ_421A9847E7927C74 ON users_user (email)');  
}

```

```shell
php bin/console doctrine:migrations:migrate -n
```

8) Укажем данные для refresh_token в его файле конфигурации gesdinet_jwt_refresh_token.yaml

```yaml
gesdinet_jwt_refresh_token:  
  refresh_token_class: App\Users\Domain\Entity\RefreshToken  
  ttl: 2592000 # 30 days время жизни
  token_parameter_name: refreshToken # переопределяет имя свойства с рефреш токеном, которое получается из объекта Request
```

9) Воспользуемся командой из документации бандла access токена, для генерации SSL ключа. Данная команда позволяет создать 2 ключа - приватный и публичный. Это необходимо для реализации асимметричного JWT.

```shell
php bin/console lexik:jwt:generate-keypair
```

По итогу выполнения этой команды в директории config создается директория jwt с двумя ключами, а именно - файлами public.pem и private.pem.

## Программная реализация JWT

1) Создадим общий интерфейс авторизованного пользователя. Создадим его в общей директории Shared/Domain/Security.
Данный интерфейст будет реализовавывать еще два это UserInterface и PasswordAuthenticatedUserInterface.

```php
<?php  
  
declare(strict_types=1);  
  
namespace App\Shared\Domain\Security;  
  
use Symfony\Component\Security\Core\User\PasswordAuthenticatedUserInterface;  
use Symfony\Component\Security\Core\User\UserInterface;  
  
/**  
 * Интерфейс авторизованного пользователя */interface AuthUserInterface extends UserInterface, PasswordAuthenticatedUserInterface  
{  
    /**  
     * Получение идетнификатора пользователя     */    public function getUlid();  
  
    /**  
     * Получение почты пользователя     */    public function getEmail();  
}
```

2) Определим интерфейс сервиса, который будет возвращать информацию о текущем авторизованном пользователе. Создадим его в той же директории, что в п.1.

```php
<?php  
  
declare(strict_types=1);  
  
namespace App\Shared\Domain\Security;  
  
/**  
 * Интерфейс возвращающий информацию о текущем авторизованном пользователе */interface UserFetcherInterface  
{  
    public function getAuthUser(): AuthUserInterface;  
}
```

3) Добавим реализацию этого интерфейса на инфраструкторном уровне

```php
<?php  
  
namespace App\Shared\Infrastructure\Security;  
  
use App\Shared\Domain\Security\AuthUserInterface;  
use App\Shared\Domain\Security\UserFetcherInterface;  
use Symfony\Component\Security\Core\Security;  
use Webmozart\Assert\Assert;  
  
/**  
 * Получает информацию об авторизованном пользователе */class UserFetcher implements UserFetcherInterface  
{  
    public function __construct(  
        private readonly Security $security  
    )  
    {  
    }  
  
    public function getAuthUser(): AuthUserInterface  
    {  
        /** @var AuthUserInterface $user */  
        $user = $this->security->getUser();  
  
        Assert::notNull(  
            $user,  
            'User not found, check security access list'  
        );  
        Assert::isInstanceOf(  
            $user,  
            AuthUserInterface::class,  
            sprintf(  
                'Invalid user type %s',  
                \get_class($user)  
            )  
        );  
  
        return $user;  
    }  
}
```

4) В модуле пользователей необходимо добавить реализацию интерфейса авторизованного пользователя в сущность

```php
<?php  
  
declare(strict_types=1);  
  
namespace App\Users\Domain\Entity;  
  
use App\Shared\Domain\Security\AuthUserInterface;  
use App\Shared\Domain\Service\UlidService;  
  
class User implements AuthUserInterface  
{  
    private string $ulid;  
    private string $email;  
    private string $password;  
  
    public function __construct(string $email, string $password)  
    {  
        $this->ulid = UlidService::generate();  
        $this->email = $email;  
        $this->password = $password;  
    }  
  
    public function getUlid(): string  
    {  
        return $this->ulid;  
    }  
  
    public function getEmail(): string  
    {  
        return $this->email;  
    }  
  
    public function getPassword(): string  
    {  
        return $this->password;  
    }  
  
    /**  
     * Возвращает информацию о ролях пользователя 
     */
     public function getRoles(): array  
    {  
        return [  
            'ROLE_USER',  
        ];  
    }  
  
    /**  
     * Очищает чувствительные данные (например пароль)
     * Т.к. он реализуется в сущности, то использовать не рекомендуется (есть угроза удаления пользовательского пароля)     
     */    
     public function eraseCredentials()  
    {  
        // TODO: Implement eraseCredentials() method.  
    }  
  
    /**  
     * Возвращает электронную почту в качестве идентификатора
     */
     public function getUserIdentifier(): string  
    {  
        return $this->getEmail();  
    }  
}
```

5) Реализуем механизм хеширования пароля пользователя.
Лучше всего хеширование поручить сущности, чтобы никто не смог забыть его захешировать.
Добавим сеттер пароля в сущность

```php
/**  
 * Сеттер пароля с хешированием
 */
public function setPassword(string $password, UserPasswordHasherInterface $passwordHasher): void  
{  
    $this->password = $passwordHasher->hash($this, $password);  
}
```

Создадим интерфейс хеширования пароля, и его реализацию

```php
<?php  
  
declare(strict_types=1);  
  
namespace App\Users\Domain\Service;  
  
use App\Users\Domain\Entity\User;  
  
/**  
 * Интерфейс хеширования пароля 
 */
interface UserHasherInterface  
{  
    public function hash(User $user, string $password): string;  
}


<?php  
  
namespace App\Users\Infrastructure;  
  
use App\Users\Domain\Entity\User;  
use App\Users\Domain\Service\UserHasherInterface;  
use Symfony\Component\PasswordHasher\Hasher\UserPasswordHasherInterface as BaseUserHasherInterface;  
  
class UserPasswordHasher implements UserHasherInterface  
{  
    public function __construct(  
        private readonly BaseUserHasherInterface $passwordHasher  
    )  
    {  
    }  
  
    public function hash(User $user, string $password): string  
    {  
        return $this->passwordHasher->hashPassword($user, $password);  
    }  
}

```

Так же возможна ситуация, когда пароль = null, например, когда у нас происходит авторизация через соц-сети. Поэтому свойству password в сущности лучше указать возможноть установки null, а так же удалить пароль из конструктора сущности.
Создавать пароль будем в фабрике пользователя.

```php
<?php  
  
declare(strict_types=1);  
  
namespace App\Users\Domain\Factory;  
  
use App\Users\Domain\Entity\User;  
use App\Users\Domain\Service\UserPasswordHasherInterface;  
  
class UserFactory  
{  
    public function __construct(  
        private readonly UserPasswordHasherInterface $passwordHasher  
    )  
    {  
    }  
  
    public function create(string $email, string $password): User  
    {  
        $user = new User($email);  
        $user->setPassword($password, $this->passwordHasher);  
  
        return $user;  
    }  
}
```



**Ключи:**
- [JWT](JWT-auth);
- [Symfony](Symfony-framework);


**Хештеги:** #Programming/Symfony/JWT
