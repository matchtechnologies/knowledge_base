## 📔 Journalctl

### Чтение

Простая команда для чтениея логов
```sh
sudo journalctl -u servicename.service
```

если у вас хранятся большие логи за большой период времени
то лучше ограчинивать запросы временным прометужтком
то есть отображать только свежие логи например

```sh
sudo journalctl -u servicename.service --since "1hour ago"
```

указывать можно любое время, в том числе и меньший
```sh
sudo journalctl -u servicename.service --since "3min ago"
```