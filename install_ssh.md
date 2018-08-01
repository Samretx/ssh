# ssh
# настройка ssh по ключу с клиента на сервер
# ubuntu 16.04 desktop > ubuntu 14.04 server
#-------------------------------------------------------
# 1. Генерируем на стороне клиента
# Сначало создаем дирректорию для генерации

mkdir /sshkeys

# Генерируем ключи

ssh-keygen -t rsa -b 4096 -f id_rsa

# Задаем пароль для ключа

# Вводим ls -l, должно быть id.rsa и id.rsa.pub. pub - кидаем на сервер

# ------------------------------------------------------
# 2. Закидываем ключ на сервак

ssh-copy-id -i id_rsa.pub root@"ip-adress servera"

# проверяем кинулся ли файл
# проверяем на сервере:

nano /.ssh/authorized_keys

# проверяем на клиенте:

nano /sshkeys/id_rsa.pub

# Добавляем ключ rsa клиенту
ssh-add ~/sshkeys/id_rsa -l


# Сравниваем
# ------------------------------------------------------
# 3. Настраиваем конфиг на сервере

nano /etc/ssh/sshd_config

# раскоментить следующие и изменить следующие строки
Repmitrootlogin yes

RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile		.ssh/authorized_keys

PasswordAuthentication no

# перед рестартом ssh желательно дополнительно открыть сервер через ssh, чтобы если что, вернуть вход по паролю

# рестартуем сервер
 service ssh restart
# ------------------------------------------------------
# FIX IT

# 1) Почистить уже вбитые серваки на клиенте

rm -rf /.ssh/known_hosts

# 2) Проверка наличия паблик ключа на сервера

ssh root@"ip-adrees" -v

# Нужно найти строку:

Authentication that can continue: publickey,password

# 3) Если не добавляется id_rsa на клиент

exec ssh-agent bash

