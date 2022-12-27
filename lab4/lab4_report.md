University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Urbaev Maxim Gennadevich
Lab: Lab4
Date of create: 18.12.2022
Date of finished: 27.12.2022
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

## 2. Implementing Basic Forwarding.

Для выполнения первого задания был запущен файл basic.p4. Пинг не проходит между свичами.

<img src = https://user-images.githubusercontent.com/67152968/208311021-22902193-a857-4cd8-bb1b-d8e6738dc775.png width=500>

Была начата работа над дополнением файла при помощи документации: https://p4.org/p4-spec/docs/P4-16-v-1.2.3.html#sec-vss-arch  
<img src = https://user-images.githubusercontent.com/67152968/208312288-9e20b036-0b1e-4d43-8a57-243e185f8f44.png width=400>

Далее были изменены секции Ingress Processing и Deparser.

В результате пинг сигнал проходит между h1 и h2ю

<img src = https://user-images.githubusercontent.com/67152968/208313315-1e4af043-ddbf-424b-81dd-7148924b8941.png width=500>

## 3. Implementing Basic Tunneling.

В директориии ~/tutorials/exercises/basic_tunnel требовалось изменить файл basic_tunnel.p4

Были внесены изменения в файл basick_tunnel.p4. Далее в minilet была выполнена команда <code>xterm h1 h2</code>.

В результате были открыты два терминала:

![image](https://user-images.githubusercontent.com/67152968/209218428-6badda2e-b9d3-46f7-ae24-ac43771d3b4e.png)

На h2 был поднят сервер командой <code>./receive.py</code>.  
На h1 была выполнена команда <code>./send.py 10.0.2.2 "P4 is cool"</code> для отправки сообщения на h2.

![image](https://user-images.githubusercontent.com/67152968/209219555-ad519be5-b4d3-4470-8886-8bd2a9c5ed3d.png)

Указав флаг --dst_id 2, но указав ip третьей машины пакет все равно дойдет до h2:  

![image](https://user-images.githubusercontent.com/67152968/209220740-39c31771-2767-4129-9703-cd703c98f34d.png)

## 4. Вывод
В ходе лабораторной работы были выполнены управжнения по реализации перенаправления пакетов и туннелирования при помощи языка программирования P4. 

## 5. Схемы связи
### Первое задание
![image](https://user-images.githubusercontent.com/67152968/209221304-2e2c082f-9654-41ac-9e80-50f99584e363.png)

---

### Второе задание
![image](https://user-images.githubusercontent.com/67152968/209221252-445a0153-4c32-4c07-9b20-274b794768c5.png)

