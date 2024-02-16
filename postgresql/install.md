## 🐘 PostgreSQL

### Установка PostgreSQL на Ubuntu

Набираем команду и соглашаемся на предложения

```sh
sudo apt-get update
sudo apt-get install -y postgresql
```

набираем

```sh
sudo -u postgres psql template1
```

```sql
ALTER USER postgres with encrypted password 'postgres';
```

```sh
sudo vi /etc/postgresql/9.3/main/pg_hba.conf
```

меняем на
```
local   all         postgres                          trust
```

осталось
```sh
sudo /etc/init.d/postgresql restart
```

создаем пользователя по инструкции [🐘 PostgreSQL > Создание пользователя](create_user.md)

подключиться по инструкции [🐘 PostgreSQL > Подключение к БД](connect.md)

у вас может быть проблема с кодировкой, проверьте что ответ на запрос такой, иначе воспользутейсь инструкцией [## 🐘 PostgreSQL > Возможные ошибки > Настройка кодировки](./emergency.md#настройка-кодировки)

```sql
xx=# show server_encoding;
 server_encoding
-----------------
 UTF8
(1 row)
```

Если используете PostGIS то еще
нужно установить для Ubuntu 20.04
```sh
sudo apt-get install -y postgis postgresql-12-postgis-3
```

подключиться и выполните
```sql
CREATE EXTENSION postgis;
```

в теории потом должно помочь команды ниже от postgres к бд, но тут что то не так
```sql
GRANT ALL PRIVILEGES ON DATABASE dbname TO username;
GRANT USAGE ON SCHEMA public TO username;
```

https://stackoverflow.com/questions/61157620/create-extension-postgis-fails
https://stackoverflow.com/questions/22483555/postgresql-give-all-permissions-to-a-user-on-a-postgresql-database

если хотите пока забить то так
```sql
ALTER USER username with SUPERUSER CREATEDB;
```

но если не хотите забить
получается в миграциях и дампе не должно быть включения расширений
то есть для этого используется суперюзер, а вот для работы уже обычные
в теории этого можно достичь если убрать из миграций, либо добавить там проверки не включать если уже есть
и бекап делать от обычного пользователя
но все равно будут возможны другие ошибки которые тогда надо будет разбирать отдельно