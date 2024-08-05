# SSH linux setup

**Ключи:**
- Обратные:
	- [Linux](linux);

**Хештеги:**
- #Linux

## Создать и передать SSH ключ на удаленный сервер


1) Сгенерировать ключ на локальной машине в дирекотрии .ssh (при необходимости создать такую)

```shell
mkdir -p ~/.ssh
chmod 700 ~/.ssh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Две последние опции не обязательны. При создании можно оставить дефолтное имя или выбрать своё, а так же указать passphrase.

2) Добавить публичный ключ на сервер

```shell
cat ~/.ssh/id_rsa.pub
```

Выведенный ключ необходимо скопировать, затем идем на удаленный сервер.

```shell
ssh user@remote_server
mkdir -p ~/.ssh
chmod 700 ~/.ssh echo "ваш_публичный_ключ" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

3) Выходим с удаленного сервера и пробуем подключиться к нему по ssh. Должно получиться подключение без пароля.

```shell
ssh user@remote_server
```

## Если не удается склонировать репозиторий хотя ключ есть

1) Активируем SSH агент

```shell
eval $(ssh-agent -s)
```

2) Добавляем приватный ключ

```shell
ssh-add ~/.ssh/your_privete_key
```

3) Клонируем

```shell
git clone $DEPLOY_DATA_SRC $DEPLOY_VARS_DIR
```