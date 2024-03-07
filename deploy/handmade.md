## 🚀 Deploy

### Ручной деплой Ruby on Rails

**📔 Оглавление**
* [Настройка Rails](#настройка-rails)
* [Настройка Nginx](#настройка-nginx)
* [Делаем запуск через Systemd](#делаем-запуск-через-systemd)
* [Повторный деплой](#повторный-деплой)

Для деплоя Ruby on Rails приложения
установим на сервер на Ubuntu вручную
по инстуркциям по ссылкам ниже
* [Nginx](../nginx/install.md)
* [Git](../git/install.md)

и создаете на сервере нового пользователя deployer по инструкции [🚀 Deploy > Создаем на сервере нового пользователя deployer](deployer.md)


#### Настройка Rails

Устанавливаем под новым пользователем по инструкции [💎 Ruby > Установка rbenv](../ruby/install.md)

Предположим что вы уже используете систему контроля версий Git,
и у вас уже есть удаленный репозиторий где размещен код проекта.
Поэтому теперь зайдем на сервер и вытяним код на тачку проекта
для этого подойте каталог `/opt`
а так же лучше сразу сделать подпапку, подпапки:
```sh
root@ubuntu:~$ sudo su deployer
$ bash
deployer@ubuntu:~$ cd /opt
deployer@ubuntu:/opt$ sudo mkdir xx_backend
deployer@ubuntu:/opt$ sudo chown deployer xx_backend
deployer@ubuntu:/opt$ cd xx_backend
deployer@ubuntu:/opt/xx_backend$ mkdir releases
deployer@ubuntu:/opt/xx_backend$ cd releases
deployer@ubuntu:/opt/xx_backend/releases$ git clone --branch master https://gitlab.com/xx/xx_backend.git /opt/xx_backend/releases/202401130203
```

и сразу делаем ссылку для приложения
```sh
ln -s /opt/xx_backend/releases/202401130203 /opt/xx_backend/current
```

прежде чем устанавливать гемы, могут понадобиться зависимости
например для корректной установки гема `psych` надо установить
```sh
sudo apt-get install -y libyaml-dev
```

а для гема `pg` 
```sh
sudo apt-get install -y libpq-dev
```

для гема `unf_ext`
```sh
sudo apt-get install -y g++
```

перейдем туда и установим гемы
```sh
deployer@ubuntu:/opt$ cd xx_backend/current
deployer@ubuntu:/opt/xx_backend/current$ bundle
```

если необходимо PostgreSQL, то воспользуйтесь инстуркцией  [🐘 PostgreSQL > Установка PostgreSQL на Ubuntu](../postgresql/install.md)

Меняем данные для подключения к бд `config/database.yml`
```yaml
production:
  <<: *default
  host: localhost
  database: xx
  username: xx
  password: xx
```

запускаем консоль проекта
```sh
RAILS_ENV=production bundle e rails c
```

выполняем миграции
```sh
RAILS_ENV=production bundle e rails db:migrate
```

надо создать
```sh
vim config/master.key 
```

и прописать там содержимое локального файла из `.gitignore`
```sh
cat config/master.key 
```

можете проверить по инструкции [💎 Ruby > Ручной запуск puma без nginx](../ruby/hand_run_puma_without_nginx.md)

#### Настройка Nginx

Правим файл
```sh
sudo vim /etc/nginx/sites-available/default
```

```lua
upstream xx_backend {
        server unix:///opt/xx_backend/current/tmp/puma.sock;
}

server {
        listen 80;

        server_name api.xx.ru;

        proxy_set_header Host $host;

        location / {
                proxy_pass http://xx_backend;
        }
}
```

перезагружаем nginx

```sh
sudo service nginx restart
```

#### Делаем запуск через Systemd

Делаем запуск по инструкци [🔧 Systemd > Как настроить запуск через systemctl](../systemd/start.md)

#### Повторный деплой

После того как запушили в git
заходим на сервер под юзером deployer
```sh
sudo su deployer
bash
```

клонируем в новый каталог
```sh
git clone --branch master https://gitlab.com/xx/xx_backend.git /opt/xx_backend/releases/202401170118
```

переходим туда и ставим гемы
```sh
cd /opt/xx_backend/releases/202401170118
bundle
```

надо создать ключ для секретов
```sh
vim config/master.key 
```

прописать там содержимое локального файла из `.gitignore`
```sh
cat config/master.key 
```

выполните миграции, если появились новые
```sh
RAILS_ENV=production bundle e rails db:migrate
```

меняем ссылку
```sh
rm /opt/xx_backend/current
ln -s /opt/xx_backend/releases/202401170118 /opt/xx_backend/current
```

перезагружаем nginx
```sh
sudo service nginx restart
```

и сам проект
```sh
sudo systemctl restart xx.target
```

проверям логи для сервиса `xx-web.service` по инструкции [📔 Journalctl > Чтение](../journalctl/read.md)
