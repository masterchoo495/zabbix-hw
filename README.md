# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Никита Роща`

---

### Задание 1

Установите Zabbix Server с веб-интерфейсом.

1. Установка базы данных PostgreSQL  
*sudo apt install postgresql*

2. Установка и распаковка репозитория  
*wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest+ubuntu24.04_all.deb*  
*dpkg -i zabbix-release_latest+ubuntu24.04_all.deb*  
*apt update*  

3. Установка заббикс сервера, веб-интерфейса  
*apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts*

4. Создание базы данных и пользователя  
*sudo -u postgres createuser* --pwprompt zabbix  
*sudo -u postgres createdb* -O zabbix zabbix

5. Импорт данных на сервер Zabbix  
*zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz* | sudo -u zabbix psql zabbix

6. Настройка пароля бызы данных  
В файле */etc/zabbix/zabbix_server.conf* нужно расскоментировать параметр **DBPassword=** и указать заданный при создании пользователя пароль

7. Перезапуск и добавление в автозагрузку zabbix-server и apache2  
*systemctl restart zabbix-server apache2*  
*systemctl enable zabbix-server apache2*

Скриншот авторизации в админке:
![alt text](https://github.com/masterchoo495/zabbix-hw/blob/main/img/img001.png)

---

### Задание 2

Установите Zabbix Agent на два хоста.

В качестве используемых для мониторинга хостов использую две отдельных виртуальных машины. Выполняю действия ниже на обеих виртуалках.

1. Установка репозитория Zabbix  
*wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest+ubuntu24.04_all.deb*  
*dpkg -i zabbix-release_latest+ubuntu24.04_all.deb*  
*apt update*  

2. Установка Zabbix агента  
*apt install zabbix-agent*

3. Правка конфига с указанием сервера, с которого агент допускает подключение  
![alt text](https://github.com/masterchoo495/zabbix-hw/blob/main/img/img003.png)  

4. Рестарт службы агента для применения изменений в конфиге  
*systemctl restart zabbix-agent*  

Скриншот раздела Configuration > Hosts  
![alt text](https://github.com/masterchoo495/zabbix-hw/blob/main/img/img002.png)  

Скриншот лога zabbix agent  
![alt text](https://github.com/masterchoo495/zabbix-hw/blob/main/img/img004.png)  

Скриншот раздела Monitoring > Latest data для первого хоста
![alt text](https://github.com/masterchoo495/zabbix-hw/blob/main/img/img005.png)  

Скриншот раздела Monitoring > Latest data для второго хоста
![alt text](https://github.com/masterchoo495/zabbix-hw/blob/main/img/img006.png)  



