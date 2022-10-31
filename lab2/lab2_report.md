University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Urbaev Maxim Gennadevich
Lab: Lab2
Date of create: 20.10.2022
Date of finished: 
___

## Цель работы

С помощью Ansible настроить несколько сетевых устройств и собрать информацию о них. Правильно собрать файл Inventory.

## Ход работы
#### 1. Установка второго CHR и второго openvpn клиента на нем.

В результате выполнения этого пункта, по аналогии с первой лабораторной работой, имеем 2 виртуальные машины, настроенные как OpenVPN client.

![image](https://user-images.githubusercontent.com/67152968/197190209-a8e2cf5e-2ddd-467e-bd68-de75162ce295.png)


#### 2. Ansible.

2.1. Нужно создать файл hosts.ini в /etc/ansible в котором разместим информацию о локальных ip двух CHR.

<img src="https://user-images.githubusercontent.com/67152968/197200806-00d96dc3-3726-4978-a4e9-67093c3df7e5.png" width="500"/>


2.2. Настройка SSH соединения для виртуальных машин.

Через winbox (можно и через консоль chr) настроим ssh.

<img src="https://user-images.githubusercontent.com/67152968/197198456-4ee617d5-a7d8-4a49-904f-2c83d3785996.png" width="400"/>

В VirtualBox добавим NAT соединение и пропишем SSH с портами.

<img src="https://user-images.githubusercontent.com/67152968/197198533-aeac78ef-ac45-417e-a5ed-a4449098384f.png" width="400"/>

Проверить подключение можно командой на сервере:
<code>ssh -p 22 admin@172.27.244.8</code>
После чего потребуется ввести установленый на CHR пароль.

Установим sshpass для решения ошибки '“to use the ssh connection type with passwords, you must install the sshpass program'.
(sshpass is a command-line tool that allows you to do non-interactive ssh login.) #конечно можно и ssh ключи закинуть.

<code>sudo apt install sshpass</code>

2.3. Playbook

Создадим первый playbook.yml файл в /etc/ansible. В качесте проверки соединения выведем информацию о роутерах с помощью "/system resource print". Также запишем команды, требующиеся для задания.

Настройка NTP: https://smartadm.ru/mikrotik-ntp-server-sntp-client/

Настройка OSPF: https://itproffi.ru/ospf-mikrotik-polnaya-instruktsiya-po-nastrojki-ospf-na-mikrotik/

<img src="https://user-images.githubusercontent.com/67152968/199102670-560a7f3c-52bc-4604-b1b0-a44bc1b12061.png" width="700"/>

Результат выполнения playbook:

<img src="https://user-images.githubusercontent.com/67152968/199105514-1852c8bc-8bf5-4657-8664-bda332d4f6c7.png" width="700"/>

<img src="https://user-images.githubusercontent.com/67152968/199106552-c673c106-03e9-4037-86b5-0209964438a6.png" width="700"/>










