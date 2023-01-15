
# Стратегическое проектирование (Stratigic design)


## Задачи

- Исследование домена;
- Построение коммуникации;
- Определение концепции;
- Выработка единой терминологии;
- Определение границ поддоменов;
- Определение подобластей.

## Концепции

Это то с чего начинается проектирование предметной области. Необходимо собрать знания и материалы о сущностях предметной области, о её событиях.
Источниками концепций могут служить:

- Эксперты;
- Техническое задание;
- Событийный штурм (event storming);
- Материалы (книги, статьи и пр.).

## Событийный штурм

Методика быстрого проектирования с вовлечением экспертов предметной области и технических экспертов. В ходе него, вышеупомянутые люди накидывают события, происходящие в предметной области.

1) Необходимо выявлять события (создание пользователя, создание навыка, создание профессии и др.)
2) Затем необходимо выявить сущности и агрегаты, связанные с этими событиями.

Сейчас концентрация идет на бизнес процессы. Инфраструктурные и прочие задачи пока не рассматриваются, есть лишь следование предметной области.

![[Pasted image 20221127224944.png]]

Оранжевые стикеры - события;
Желтые стикеры - агрегаты

3) После выделения всех концепций мы получаем большое облако с событиями сущностями и агрегатами. Далее необходимо все эти сущности кластеризовать по огрниченным контекстам (bounded context, поддомены), где внутри контекстов сущности и события связаны друг с другом по смыслу.

![[Pasted image 20221127225610.png]]

Ограниченные контексты по навыкам, компаниям, тестированию и др..

![[Pasted image 20221127225807.png]]


4) Необходимо определить связанность контекстов (какие связи между контекстами будут).

![[Pasted image 20221127231308.png]]

Подробнее можно посмотреть с главе со [слоеной архитектурой](Onion-architecture) и [модульным монолитом](Monolith-first).
Для снижения связанности между контекстами можно выделить Anti-Corruption Layer с различными адаптерами.

У проекта может быть общее ядро (Shared kernel), его принято использовать, если несколько команд используют  общую модель в коде. В этом случае две команды работают над общей частью в рамках единого языка и любые изменения согласовываются в двухстороннем порядке.

Примером общего ядра может служить общая директория при разработке модульного монолита, куда выносятся общие интерфейсы, сервисы и утилиты, используемые сразу в нескольких модулях.

Так же существует связь между ограниченными контекстами типа [**Партнерство (Partnership)**](Partnership).

5) Разбить предметную область на три подобласти (subdomain):
	- Смысловое ядро (Core domain) - самая важная подобласть, несет наибольшую ценность для бизнеса, разработку функционала этой подобласти должны вести наиболее сильные участники разработки, на нее нужно направить самые ценные ресурсы компании;
	- Вспомогательная подобласть (Supporting subdomain) - ограниченные контексты и функционал, который не несет особой ценности для бизнеса, ее разработку можно отдать второстепенным командам или аутсорсу;
	- Универсальная подобласть (Generic subdomain) - функционал, который можно приобрести в виде готового решения (интеграция).

Каждая подобласть несет в себе определенный стратегический характер.

Например, для функционала тестирования навыков сотрудников компании.
Самое важное смысловое ядро - это навыки. Вокруг них все крутится. Вспомогательная область - это компании, пользователи, тестирование.
Универсальная подобласть - контент, файлы и оплата.

![[Pasted image 20230107145715.png]]

Здесь завершается стратегическое проектирование.

**Ключи:**
- [DDD](DDD);

**Хештеги:** #Programming/Arch/DDD/Strategic
