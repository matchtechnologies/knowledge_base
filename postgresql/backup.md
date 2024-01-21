## 🐘 PostgreSQL

### Работа с backup

#### Создание backup

Для создания backup, вам нужно что бы на сервера с которого вы выполняете команду был установлен `pg_dump`

если вы хотите выполнить команду к удаленному серверу с БД то сначала установите на этот сервер PostgreSQL по инструкции [🐘 PostgreSQL > Установка PostgreSQL на Ubuntu](install.md)

можете проверить работает ли у вас подключение к базе по инструкции [🐘 PostgreSQL > Подключение к БД](connect.md)
```sh
psql -h 11.22.33.44 -U xx -d xx
```

простой пример команды создания дампа
```sh
pg_dump -Fc xx -h 11.22.33.44 -U xx > db.dump
```

еще можно так, но не понятно как восстановить
```sh
pg_dump xx -h 11.22.33.44 -U xx > db.sql
```

подробнее https://www.postgresql.org/docs/current/app-pgdump.html

теперь можно скачать дамп по инстуркции [🚚 SCP > Скачивание](../scp/download.md)

#### Разворачивание backup

Если у вас уже есть backup с другого сервера и вы уже его скачали, теперь надо отправить его на нужный сервер по инстуркции [🚚 SCP > Загрузка](../scp/upload.md)

создаем бд с любыми именем,
но обязательно создать пользователя с таким же именем как по которой был создан дамп
то есть предварительно вам скорее всего нужно создать пользователя по инструкции [🐘 PostgreSQL > Создание пользователя](create_user.md)
```sh
CREATE DATABASE databasename OWNER username;
```

подключитесь к ней и выдайте юзеру на нее права
```sh
root@vm2601525:~# psql -U postgres -h localhost -d databasename
Password for user postgres: 
psql (12.17 (Ubuntu 12.17-0ubuntu0.20.04.1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.

databasename=# GRANT ALL ON SCHEMA public TO xx;
GRANT
```

если у вас возникли какие то проблемы то можете удалить и потом создать заново
```sql
DROP DATABASE databasename;
```

простой пример команды восстонавления дампа
```sh
pg_restore -Fc -d "postgresql://username:password@127.0.0.1:5432/databasename" db.dump
```

подробнее https://www.postgresql.org/docs/current/app-pgrestore.html