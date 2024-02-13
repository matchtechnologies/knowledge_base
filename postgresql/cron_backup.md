## 🐘 PostgreSQL

### Автоматическое создание backup по расписанию

**📔 Оглавление**
* [Подготовка команды](#подготовка-команды)
* [Создание скрипта](#создание-скрипта)
* [Настройка cron](#настройка-cron)

#### Подготовка команды

Ознакомьтесь с инструкциями 
[🐘 PostgreSQL > Работа с backup > Создание backup](./backup.md)
[📇 Bash > Вывод даты в консоль](../bash/date.md)

сделайте бекап с его датой создания
```sh
pg_dump -Fc dbname -U postgres > `date +"/root/%Y%m%d%H%M%S.dump"`
```

проверье его
```sh
ls -lahS /root
less -R /root/outputyourdate.dump
```

#### Создание скрипта

Ознакомьтесь с инструкцией [📇 Bash > Создание и выполнение скрипта](../bash/script.md)

создайте скрит `backup.sh` с командой выше по инструкции
```sh
echo "Creating a backup"
pg_dump -Fc dbname -U postgres > `date +"/root/%Y%m%d%H%M%S.dump"`
```

#### Настройка cron

Ознакомьтесь с инструкцией [⏰ Cron > Настройка запуска](../cron/setting.md)

наберите команду
```sh
crontab -e
```

пропишите там
```sh
0 0 * * * /root/beckup.sh
```