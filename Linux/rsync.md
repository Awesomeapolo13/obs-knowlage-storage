
# Команда rsync

**Rsync** -  это remote sync или удаленная синхронизация, является средством удаленной и локальной синхронизации файлов. Он использует алгоритм, который минимизирует объем копируемых данных, перемещая только те части файлов, которые были изменны.

Утилита Rsync используется для синхронизации файлов и каталогов между удаленными серверами, а также на локальной машине на базе операционных систем семейства Linux.

*Пример:*
```shell
rsync -azrqP -e "ssh -p $PORT" .env.deploy $USER@$IP:$DIR/$CI_PROJECT_NAME/.env
```

Команда выше делает следующее:
1) rsync - вызов утилиты;
2) -a - режим архива (позволяет выполнять рекурсивную синхронизацию, сохраняя символические ссылки и пр.);
3) -z - сжимает данные файла во время передачи;
4) -r - рекурсивная сихронизация;
5) -q - подавлять сообщения без ошибок;
6) -P - то же что и --partial --progress, а именно - сохранять частично переданные файлы и показывать прогресс во время передачи соответственно;
7) -e - указывает удаленную оболочку для использования.

Ключи:
- [Остановка процессов](Killing-processes);
- [Команда rsync](rsync);

**Хештеги:**
- #Linux