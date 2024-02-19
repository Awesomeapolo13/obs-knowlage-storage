
# Deptrac

**Ключи:**
- [Статические анализаторы кода](static-code-analizers);

**Хештеги:** #Programming/PHP/CodeAnalizer

## Определение

Инструмент, позволяющий выявлять проблемы, связанные с архитектурными ограничениями и контролировать взаимоотношения между классами.
Он необходим, чтобы проект не превратился в большой ком грязи из-за сильной свяханности между классами и модулями.

## Установа и настройка

Установка через композер

```shell
composer require qossmic/deptrac-shim
```

Создать два файла deptrac-layers.yaml и deptrac-modules.yaml.

Конфигурационный файл deptrac-layers.yaml - будет проверять нарушение слоев, deptrac-modules.yaml - 


## Настройка deptrac-layers

Согласно слоеной архитектуре, у нас может быть три слоя доменный, приложения и инфраструктурный. Доменный слой независит ни от кого, слой Приложения может зависить только от доменного, Инфраструктурный - от любого из перечисленных выше.

```yaml
parameters:  
  # Директория со всеми нашими модулями.  
  paths:  
    - ./src  
  # Слои  
  layers:  
    - name: Domain  
      collectors:  
        - type: directory  
          regex: /src/\w+/Domain/.*  
  
    - name: Application  
      collectors:  
        - type: directory  
          regex: /src/\w+/Application/.*  
  
    - name: Infrastructure  
      collectors:  
        - type: directory  
          regex: /src/\w+/Infrastructure/.*  
  # Правила  
  ruleset:  
    Domain:  
    Application:  
      - Domain  
    Infrastructure:  
      - Domain  
      - Application
```

Для запуска анализа выполнить команду:

```shell
vendor/bin/deptrac analyse --config-file=deptrac-layers.yaml
```
где в опцию --config-file нудно передать путь до файла конфигурации.


## Настройка deptrac-modules

Позволит найти проблемы сильной связанности можду модулями.
Модули не должны знать ничего друг о друге и не иметь зависимостей, кроме как через классы адаптеров.

```yaml
parameters:  
  # Директория со всеми нашими модулями.  
  paths:  
    - ./src  
  exclude_files:  
    - '#.*\/src\/.*\/Infrastructure\/Adapter\/.*#'  
  # Слои - директории с модулями  
  layers:  
    - name: Shared  
      collectors:  
        - type: directory  
          regex: /src/Shared/.*  
  
    - name: Users  
      collectors:  
        - type: directory  
          regex: /src/Users/.*  
  
    - name: Recipes  
      collectors:  
        - type: directory  
          regex: /src/Recipes/.*  
  # Правила (адаптеры не включаем, т.к. они могут иметь любые зависимости)  
  ruleset:  
    Users:  
      - Shared
```

Для запуска анализа выполнить команду:

```shell
vendor/bin/deptrac analyse --config-file=deptrac-modules.yaml
```
где в опцию --config-file нудно передать путь до файла конфигурации.
