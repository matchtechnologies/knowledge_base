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

создаем бд с таким же именем как по каторой был создан дамп
```sh
CREATE DATABASE databasename OWNER username;
```

простой пример команды восстонавления дампа
```sh
pg_restore -Fc -d "postgresql://username:password@127.0.0.1:5432/databasename" db.dump
```

подробнее https://www.postgresql.org/docs/current/app-pgrestore.html