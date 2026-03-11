# Домашнее задание к занятию «Система мониторинга Zabbix»
**Выполнил:** Чехлов Михаил



## Задание 1: Установка Zabbix Server

### Использованные команды

```bash
# Обновление списка пакетов
sudo apt update

# Установка PostgreSQL
sudo apt install postgresql postgresql-contrib

# Скачивание и установка пакета репозитория Zabbix (версия 6.4 для Debian 11)
wget https://repo.zabbix.com/zabbix/6.4/debian/pool/main/z/zabbix-release/zabbix-release_6.4-1+debian11_all.deb
sudo dpkg -i zabbix-release_6.4-1+debian11_all.deb

# Обновление списка пакетов после добавления репозитория Zabbix
sudo apt update

# Установка компонентов Zabbix и Apache
sudo apt install zabbix-server-pgsql zabbix-frontend-php php8.2-pgsql zabbix-apache-conf zabbix-sql-scripts

# Создание базы данных PostgreSQL (выполняется в консоли psql)
# sudo -u postgres psql
# CREATE USER "zabbix" WITH PASSWORD 'ваш_надёжный_пароль';
# CREATE DATABASE "zabbix" OWNER "zabbix";
# \q

# Импорт схемы базы данных
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

# Настройка Zabbix‑сервера: редактирование /etc/zabbix/zabbix_server.conf
# DBPassword=ваш_надёжный_пароль

# Настройка PHP для веб‑интерфейса: редактирование /etc/zabbix/apache.conf
# php_value date.timezone Europe/Moscow

# Запуск и включение служб
sudo systemctl restart zabbix-server apache2
sudo systemctl enable zabbix-server apache2

# Проверка статуса служб
sudo systemctl status zabbix-server
sudo systemctl status apache2
