
# Многослойная архитектура

Это подход к архитектуре приложения, когда бизнес логика изолирована от деталей инфраструктуры.

![[Pasted image 20221022143159.png]]

То есть бизнес логика находится в самом центре и должна быть устойчива к внешним изменениям, не зависеть от фреймворка, способа хранения данных, сервисов уведомлений, логирования и др.
Доменный слой не должен зависить от внешних слоев. Внешние сложе же могут зависеть от внутренних.

Многолойная архитектура не ограничивается тремя указанными на картинке слоями и может включать другие дополнительные.

## Бизнес логикас(Domain)

Слой бизнес логики включает в себя сущности, агрегаты, события предметной области, фабрики, интерфейсы репозиториев и вспомогательные методы управления бизнес логикой.

## Прикладной слой (Application)

Буферный слой, определяющий как внешний мир может взаимодействовать с предметной областью.
Здесь находятся команды, запросы, пользовательские сценарии, DTO, сервисы прикладного слоя, обработчики событий и др.

## Инфраструктурный слой (Infrastructure)

Слой, который общается с предметной областью через пользовательские сценарии. На этом слое находится пользовательский интерфейс, API, реализация сервисов прикладного слоя, реализация репозиториев, тесты и некоторые адаптеры внешних сервисов, другие внешние конфигураторы, дата мапперы и др. необходимые нам структуры.


**Хештеги:** #Programming/Arch