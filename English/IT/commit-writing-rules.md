
# Правила написания комитов

**Ключи**
	Обратные:
	- [ENGLISH](ENGLISH);
	 

## Общие правила

1) Повелительное наклонение (Imperative): писать так, будто даешь команду.
	Заголовок коммита должен продолжать фразу «If applied, this commit will...».
	- ✅ `Add retry logic to RabbitMQ consumer`
	- ❌ `Added retry logic` / `Adding retry logic` / `Added retry logic to consumer`
2) Заголовок короткий, до 50 символов (максимум до 72).
	- С заглавной буквы
	- Без точки в конце
	- Описывает, что делает коммит
3) Пустая строка, затем тело (если нужно)
	В теле содержится объяснение зачем и почему, а то что видно в diff. Каждая строка до 72 символов.
```
Cache flight search results in Redis

Search queries hit ClickHouse on every request and take up to
2 seconds under load. Cache results for 5 minutes, keyed by the
normalized query, to reduce load on the database.
```

## Примеры

| Плохо                                              | Хорошо                                        | Почему                             |
| -------------------------------------------------- | --------------------------------------------- | ---------------------------------- |
| `fix`                                              | `Fix null pointer in ticket price calculator` | Слишком расплывчато                |
| `Fixed bug.`                                       | `Fix duplicate booking on retry`              | Прошедшее время, точка, нет сути   |
| `Some changes in repository`                       | `Extract query logic into FlightRepository`   | Не сказано, какие именно изменения |
| `Added new feature for users to be able to filter` | `Add filter by airline to search`             | Длинно и неестественно             |
| `Update`                                           | `Update Doctrine mapping for Order entity`    | Нет контекста                      |

## Полезные глаголы

`Add`, `Remove`, `Fix`, `Update`, `Rename`, `Move`, `Extract`, `Replace`, `Refactor`, `Simplify`, `Handle`, `Prevent`, `Introduce`, `Revert`.

Небольшие нюансы:
- `Fix`: исправление бага. `Prevent`: когда вы добавляете защиту от ошибки.
- `Refactor`: когда поведение не меняется.
- `Handle`: «добавить обработку» чего-то (`Handle empty response from API`).

## Conventional commits

Во многих командах используют префиксы:

```
feat: add airline filter to search
fix: prevent duplicate booking on retry
refactor: extract pricing logic into service
docs: describe RabbitMQ setup in README
test: add tests for fare calculator
chore: upgrade symfony/messenger to 7.3
```

Можно с областью: `fix(booking): prevent duplicate reservation`. Внутри всё то же правило: после двоеточия imperative, с маленькой буквы, без точки. Какой стиль выбрать, лучше смотреть по истории репозитория или принятым правилам команды.

## Типичные ошибки

- Прошедшее время вместо imperative (`Added`, `Fixed`).
- Артикли: `Fix bug in the calculator` и `Fix bug in calculator` оба допустимы. В заголовках артикли часто опускают, чтобы сократить, поэтому не переживайте из-за них.
- Слишком общие слова: `changes`, `stuff`, `updates`, `fix`.
- Описание того, **как** сделано, вместо того, **что** и **зачем** изменилось.
- Несколько несвязанных изменений в одном коммите (это уже вопрос не английского, а гигиены коммитов).

## Быстрая самопроверка

1. Подходит ли заголовок к фразе «If applied, this commit will ___»?
2. Укладывается ли он примерно в 50 символов?
3. Понял бы коллега через полгода, что и зачем изменилось?