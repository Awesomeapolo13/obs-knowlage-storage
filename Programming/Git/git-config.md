
# Конфигурация  Git

1)  Посмотреть все текущие конфигурации

```shell
git config --global --list
```

- *core.autocrlf=input* - для пользователей на *linux* и *mac* установить в input, для  *windows - true*, данный конфиг решает проблемы переноса строк для windows;

* filemode - git помечает файл как измененный, даже если изменить права на него.  Чтобы отключить это поведение, необходимо использовать этот конфиг: *git config filemode=false*;

- *merge.tool=* - задает инструмент, используемый для разрешения конфликтов при слиянии;

- *diff.tool=* - задает инструмент, используемый для просмотра изменений в истории.

2) Можно настроить алиасы для ряда команд

```shell
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global alias.last 'log -1 HEAD' 
```
Последняя команда показывает историю последнего коммита.

**Ключи:**
- [Git](Git)

**Хештеги:** #Programming/Git