University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Urbaev Maxim Gennadevich
Lab: Lab2
Date of create: 20.10.2022
Date of finished: 10.11.2022
___

## Цель работы

С помощью Ansible настроить несколько сетевых устройств и собрать информацию о них. Правильно собрать файл Inventory.

## Ход работы
#### 1. Установка второго CHR и второго openvpn клиента на нем.

В результате выполнения этого пункта, по аналогии с первой лабораторной работой, имеем 2 виртуальные машины, настроенные как OpenVPN client.

![image](https://user-images.githubusercontent.com/67152968/197190209-a8e2cf5e-2ddd-467e-bd68-de75162ce295.png)

---
#### 2. Ansible.

2.1. Нужно создать файл hosts в /etc/ansible в котором разместим информацию о локальных ip двух CHR.

<img src="https://user-images.githubusercontent.com/67152968/199111161-90ff3fc3-9e9a-4c86-a021-fb2f73367869.png" width="500"/>



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

Результат выполнения playbook (команда ansible-playbook playbook.yml):

<img src="https://user-images.githubusercontent.com/67152968/199105514-1852c8bc-8bf5-4657-8664-bda332d4f6c7.png" width="400"/><img src="https://user-images.githubusercontent.com/67152968/199106552-c673c106-03e9-4037-86b5-0209964438a6.png" width="600"/>


2.4. Проверка на роутерах.

Проверим создался ли пользователь.

<img src="https://user-images.githubusercontent.com/67152968/199106932-4daacc66-079b-48b5-8b70-bfb9edde1dd0.png" width="900"/>

Изменение времени отобразилось в консоли на обоих роутерах.

<img src="https://user-images.githubusercontent.com/67152968/199107426-19711edb-9a4f-46be-b659-a28a04c97ded.png" width="900"/>

Для проверки OSPF в winbox перейдем в /ip/neighbors и там должен появится сосед.

<img src="https://user-images.githubusercontent.com/67152968/199107860-bec6c68f-5f1e-4aca-a8aa-57e5a789a52d.png" width="800"/>

Пропингуем роутеры в полученной локальной сети.

<img src="https://user-images.githubusercontent.com/67152968/201069090-3918db1a-1f4c-4a7b-ac47-d33a83734b64.png"/>

Для сохранения конфигурации используем команду из рисунка ниже. После этого в /files можной найти нужный нам файл и скачать на основную машину.

<img src="https://user-images.githubusercontent.com/67152968/199109381-f4e14931-d4ce-418f-ae95-288e8729c744.png" width="600"/>


___

#### 3. Схема связи

В draw.io была нарисована схема связи

<img src="https://user-images.githubusercontent.com/67152968/199112114-ebfc74d6-fe1c-4c42-923a-db391bb2a4e3.png" width="600"/>

---
#### 4. Вывод

В результате работы были настроены два роутера через ansibe с удаленного сервера. Использоване Ansible удобно для настройки одинаковых конфигураций.






