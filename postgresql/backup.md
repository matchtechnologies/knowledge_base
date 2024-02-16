## 🐘 PostgreSQL

### Работа с backup

#### Создание backup

Для создания backup, вам нужно что бы на сервера с которого вы выполняете команду был установлен `pg_dump`

если вы хотите выполнить команду к удаленному серверу с БД то сначала установите на этот сервер PostgreSQL по инструкции [🐘 PostgreSQL > Установка PostgreSQL на Ubuntu](install.md)

можете проверить работает ли у вас подключение к базе по инструкции [🐘 PostgreSQL > Подключение к БД](connect.md)

простой пример команды создания дампа
```sh
pg_dump -Fc databasename -h 11.22.33.44 -U username > db.dump
```

еще можно так, но не понятно как восстановить
```sh
pg_dump databasename -h 11.22.33.44 -U username > db.sql
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

подключитесь к базе по инструкции [🐘 PostgreSQL > Подключение к БД](connect.md)
под пользователем postgres и выдайте юзеру на нее права следующей командой
```sql
GRANT ALL ON SCHEMA public TO username;
```

еще у вас может быть проблема с кодировкой, проверьте что ответ на запрос такой, иначе воспользутейсь инструкцией [## 🐘 PostgreSQL > Возможные ошибки > Настройка кодировки](./emergency.md#настройка-кодировки)

```sql
xx=# show server_encoding;
 server_encoding
-----------------
 UTF8
(1 row)
```

в теории этого должно хватить, но у меня все равно не работает
как вариант можете дать суперюзера
```sql
ALTER USER username with SUPERUSER;
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
