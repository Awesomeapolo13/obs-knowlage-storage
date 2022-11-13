
# Реализация аутентификации по JWT в Symfony


Для реализации необходимы два бандла.
- [Бандл для работы с Access token](https://github.com/lexik/LexikJWTAuthenticationBundle)
- [Бандл для работы с Refresh token](https://github.com/markitosgv/JWTRefreshTokenBundle)

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

6) Выполним миграцию для создания таблицы рефреш токенов



**Ключи:**
- [JWT](JWT-auth);
- [Symfony](Symfony-framework);


**Хештеги:** #Programming/Symfony/JWT
