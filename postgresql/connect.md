## 🐘 PostgreSQL

### Подключение к БД

К локальному серверу под пользователем postgres
```sh
psql -U postgres -h localhost
```

к удаленному серверу под другим пользователем и к конкретной базе данных
```sh
psql -h 11.22.33.44 -U username -d databasename
```