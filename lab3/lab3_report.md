University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Urbaev Maxim Gennadevich
Lab: Lab3
Date of create: 29.11.2022
Date of finished: 
___

## Цель работы

С помощью Ansible и Netbox собрать всю возможную информацию об устройствах и сохранить их в отдельном файле.

## Ход работы
#### 1. Установка Netbox.

Установка Netbox на удаленный сервер требует установки зависимостей. В первую очередь это postgresql:

<code>apt install postgresql</code>

![image](https://user-images.githubusercontent.com/67152968/204385637-3903dc41-3174-455a-bea8-180fde78ecc8.png)

После содадим базу netbox, а также пользователя netbox со всеми привилегиями.

![image](https://user-images.githubusercontent.com/67152968/204386377-a7c028ba-07e0-417d-a867-07e948ae9587.png)

Дальше установим redis:

<code>apt install redis-server</code>

Полсе этого была выполнена установка netbox из репозитория: https://github.com/netbox-community/netbox/archive/v2.8.8.tar.gz и выполнены команды из рисунка ниже.

![image](https://user-images.githubusercontent.com/67152968/204388037-8c9ee6cb-a960-4889-bde0-a8a578df44a4.png)


---
#### 2. Ansible.

