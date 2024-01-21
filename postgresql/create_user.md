## 🐘 PostgreSQL

### Создание пользователя
Подключаемся по инструкции [🐘 PostgreSQL > Подключение к БД](connect.md)

cоздаем пользователя
```sql
CREATE USER username;
```
подробнее https://www.postgresql.org/docs/current/sql-createuser.html

даем права
```sql
ALTER USER username CREATEDB;
```

создаем бд
```sql
CREATE DATABASE databasename;
```

меняем пароль
```sql
ALTER USER username WITH PASSWORD 'password';
```

можете проверить
```sh
psql -U username -h localhost
```