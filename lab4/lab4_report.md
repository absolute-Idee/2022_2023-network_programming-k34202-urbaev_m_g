University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Urbaev Maxim Gennadevich
Lab: Lab4
Date of create: 18.12.2022
Date of finished: 
___

# Цель работы

Ознакомиться с языком программирования P4. Выполнить базовую коммутацию и туннелирование.

# Ход работы
## 1. Установка Vagrant и виртуальной машины.

При помощи vpn был скачен и установлен Vagrant. VirtualBox уже был установлен. 

Далее был клонирован репозиторий https://github.com/p4lang/tutorials

<img src = https://user-images.githubusercontent.com/67152968/208302262-e622a987-4465-4d4c-b2c8-e69c634953aa.png width=500>

После обновления VirtualBox была успешно выполнена команда <code>vagrant up</code> в директории ./tutorial/vm-ubuntu-20.04

Был выполнен вход под пользователем P4.

<img src = https://user-images.githubusercontent.com/67152968/208305560-22ac5fa9-0f18-4047-8061-8f69089d6017.png width=500>

## 2. Implementing Basic Tunneling.

Для выполнения первого задания был запущен файл basic.p4. Пинг не проходит между свичами.

<img src = https://user-images.githubusercontent.com/67152968/208311021-22902193-a857-4cd8-bb1b-d8e6738dc775.png width=500>

