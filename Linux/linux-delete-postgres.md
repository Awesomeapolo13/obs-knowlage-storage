
# Удаление Postgres

**Ключи:**
- Обратные:
	- [Linux](linux);

**Хештеги:**
- #Linux/Postgres 


1. **Остановите службу PostgreSQL:**

```shell
sudo systemctl stop postgresql
```

2. **Удалите пакеты PostgreSQL:**

```shell
sudo apt-get --purge remove postgresql postgresql-*
```

Флаг `--purge` удаляет пакеты вместе с их конфигурационными файлами.

3. **Удалите оставшиеся зависимости и ненужные пакеты:**

```shell
sudo apt-get autoremove
```

4. **Очистите локальный репозиторий пакетов:**

```shell
sudo apt-get clean
```

5. **Удалите оставшиеся данные и конфигурационные файлы (если необходимо):**

```shell
sudo rm -rf /etc/postgresql sudo rm -rf /var/lib/postgresql sudo rm -rf /var/log/postgresql
```

