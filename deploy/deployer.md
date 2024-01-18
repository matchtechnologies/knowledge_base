## 🚀 Deploy

### Создаем на сервере нового пользователя deployer

Предварительно проверьте наличие настройки
```sh
$ cat /etc/login.defs | grep CREATE_HOME
```

добавьте, если нет 
```sh
CREATE_HOME yes
```

выполните
```sh
$ sudo useradd deployer
```

в файле `/etc/sudoers` прописываем ему, что бы отключить пароль от sudo
```sh
deployer    ALL=(ALL)       NOPASSWD: ALL
```

заходим под него
```sh
sudo su deployer
bash
```