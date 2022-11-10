University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Urbaev Maxim Gennadevich
Lab: Lab1
Date of create: 20.09.2022
Date of finished: 13.10.2022
___

## Цель работы

Целью данной работы является развертывание виртуальной машины на удаленном сервере с установленной
системой контроля конфигураций Ansible и установка CHR в VirtualBox.

## Ход работы
#### 1. Установка OpenVPN Server.

На сервере (VPS от Hostens) установим Ubuntu версии 20.04. Я подключаюсь с помощью PuTTy, так как основной компьютер на windows.

<img src="https://user-images.githubusercontent.com/67152968/194852590-8faba078-0ccf-483c-88fd-ed653f803271.png" width="600"/>


На Ubuntu уже предустановлен python, так что для установки ansible нужно выполнить данные команды:

<code>apt install python3-pip</code>
  
<code>apt-get install -y python-jinja2</code> *#доп. зависимость*

<code>pip3 install ansible</code>


Для установки OpenVPN был выбран OpenVPN Access Server, так как он имеет удобный GUI в браузере и удобный механизм управления пользователями.

* ссылка на туториал: https://tonyteaches.tech/openvpn-ubuntu/
 
Команды:

<code>apt update</code>
  
<code>apt upgrade</code>
  
<code>apt install ca-certificates wget net-tools gnupg</code>
  
<code>wget -qO - https://as-repository.openvpn.net/as-repo-public.gpg | apt-key add -</code>
  
<code>echo "deb http://as-repository.openvpn.net/as/debian focal main">/etc/apt/sources.list.d/openvpn-as-repo.list</code>
  
<code>apt update</code>

<code>apt install openvpn-as</code>


После этого на Access Server GUI(ip+943 порт) от лица админа отключим TLS(/configuration/Advanced VPN), отключим поддержку UDP(/configuration/network settings),
и создадим пользователя с паролем(new profile).

<img src="https://user-images.githubusercontent.com/67152968/194856740-c0802f9c-2522-4407-bceb-b8e90f54eac2.png" width="600"/>

После этого начнется загрузка файла с расширение .ovpn в котором уже содержатся ключи и сертификаты.



#### 2. Установка CHR(RouterOS) на VirtualBox.

Образ RouterOS скачивается с сайта https://mikrotik.com/download из раздела Cloud Hosted Router(CHR) в формате vdi. *#очень важно, т.к. на RouterOS у меня были проблемы*

В VirtualBox при создании машины указывается тип - Other, версия Other/Unknown (64-bit), а в качестве хранилища указывается ранее скаченный файл в формате .vdi. 
В настройке ВМ в одном из адаптеров указывается тип подключения: Сетевой мост (Без этого не подключить WinBox)

На https://mikrotik.com/download также нужно скачать WinBox для упрощенной работы с CHR.

При первом запуске CHR указываем логин admin, а пароль пропускаем и создаем при требовании. В WinBox подключаемся по MAC адресу к CHR, логин - admin, пароль - установленный.

#### 3. Настройка OpenVPN клиента на RouterOS

Загрузим через файлы скаченный ранее .ovpn файл с сервера.
<img src="https://user-images.githubusercontent.com/67152968/194859148-7e1b2569-f05f-4264-958c-5eae892fdc12.png" width="600"/>

Вытащим сертификаты и ключ через консоль (команда <code>certificate import file-name=profile-19.ovpn</code>)

<img src="https://user-images.githubusercontent.com/67152968/194859414-39ee2479-9794-4b32-be56-ee70c1068089.png" width="400"/>

Проверить и узнать имя можно в /system/certificates

<img src="https://user-images.githubusercontent.com/67152968/194859768-67b3c96a-a0fb-4148-a439-255400e5a65b.png" width="400"/>

Дальше создадим openvpn интерфейс. В разделе PPP создадим OVPN Client в котором укажем 

* ip 
* port
* user
* password(если установлен для пользователя на сервере)
* certificate(тот у которого KT флаги)
* cipher = aes256

<img src="https://user-images.githubusercontent.com/67152968/194860586-4b916fe6-bea6-4e89-86a8-726a5bb09421.png" width="400"/>

Проверим соединение:

<img src="https://user-images.githubusercontent.com/67152968/194860863-20d8ffd9-cfbb-4832-908b-1dcd8dc5f276.png" width="400"/>


## Вывод
В ходе данной лабораторной рабоыт я научился поднимать VPN которым успешно пользуюсь на своих устройствах. Научился лучше работать с VirtualBox. Узнал как настроивать RouterOS.






