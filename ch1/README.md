# Введение

## Установка

~~~ sh
sudo ptp install ansiЫe
~~~

## Передача информации о сервере в Ansible

Ansible  управляет хостами из реестра - о умолчанию они сохраняются в файле  hosts

Например конфигурация реестра может быть указана в файле следующим образом

~~~ sh
host1 ansible_host=192.168.1.35 ansible_port=22 ansible_user=alex ansible_private_key_file=~/.ssh/id_rsa
~~~

## Конфигурация через ansible.cfg

Файл конфигурации ansible.cfg ищется  следующих местах:

1. В локации указываемой переменной ANSIBLE_CONFIG
2. В текущей директории
3. в домашней директории
4. /etc/ansible

В конфигурацию выносятся общие параметры для подключения к группе хостов

~~~ yaml
[defaults]
inventory = hosts
remote_user = alex
private_key_file = ~/.ssh/id_rsa
host_key_checktng = False
~~~
 
и тогда конфигурация инвентори упрощается до 

~~~ sh
host1 ansible_host=192.168.1.35 ansible_port=22
~~~

пример вызова команд
~~~ sh
ansible host1  -m command -a uptime                                                                     
host1 | CHANGED | rc=0 >>
 17:40:12 up 15 min,  1 user,  load average: 0.03, 0.02, 0.03
~~~

~~~ sh
ansible host1  -a "tail /var/log/dmesg"

host1 | CHANGED | rc=0 >>
[    7.612387] kernel: audit: type=1400 audit(1602437063.415:4): apparmor="STATUS" operation="profile_load" profile="unconfined" name="/usr/lib/connman/scripts/dhclient-script" pid=493 comm="apparmor_parser"
~~~