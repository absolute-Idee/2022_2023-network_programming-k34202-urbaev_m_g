University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
Year: 2022/2023
Group: K34202
Author: Urbaev Maxim Gennadevich
Lab: Lab3
Date of create: 29.11.2022
Date of finished: 08.12.2022
___

# Цель работы

С помощью Ansible и Netbox собрать всю возможную информацию об устройствах и сохранить их в отдельном файле.

# Ход работы
## 1. Установка Netbox.

Установка Netbox на удаленный сервер требует установки зависимостей. В первую очередь это postgresql:

<code>apt install postgresql</code>

![image](https://user-images.githubusercontent.com/67152968/204385637-3903dc41-3174-455a-bea8-180fde78ecc8.png)

После содадим базу netbox, а также пользователя netbox со всеми привилегиями.

![image](https://user-images.githubusercontent.com/67152968/204386377-a7c028ba-07e0-417d-a867-07e948ae9587.png)

Дальше установим redis:

<code>apt install redis-server</code>

Полсе этого была выполнена установка netbox из репозитория: [https://github.com/netbox-community/netbox/archive/v3.3.9.tar.gz](https://github.com/netbox-community/netbox/releases/tag/v3.3.9).

<code>sudo wget https://github.com/netbox-community/netbox/archive/refs/tags/v3.3.9.tar.gz</code>  
<code>sudo tar -xzf v3.3.9.tar.gz -C /opt</code>  
<code>sudo ln -s /opt/netbox-3.3.9/ /opt/netbox</code>  

После был создан пользователь:  
<code>groupadd --system netbox</code>  
<code>sudo adduser --system -g netbox netbox</code>  
<code>chown --recursive netbox /opt/netbox/netbox/media/</code>  

Далее была создана виртуальная среда и установлены все зависимости:  
<code>python3 -m venv /opt/netbox/venv</code>  
<code>source venv/bin/activate</code>  
<code>pip3 install -r requirements.txt</code>

Далее был скопирован файл конфигурации и отредактирован(в ALLOWED_HOST добавлены все, параметры Постгреса и секретный ключ, сгенерированый командой <code>python3 ../generate_secret_key.py</code>)

<code>cd netbox/netbox/</code>  
<code>cp configuration.example.py configuration.py</code>

Были выполнены миграции: <code>python3 manage.py migrate</code>  
Создан суперюзер: <code>python3 manage.py createsuperuser</code>  
Собрана статика: <code>python3 manage.py collectstatic --no-input</code>

---

Также для заупуска потребуется установить и настроить **Nginx** и **gunicorn**

Для настройка gunicorn нужно скопировать файл.  
<code>sudo cp /opt/netbox/contrib/gunicorn.py /opt/netbox/gunicorn.py</code>

После этого требуется запустить systemd службу:  
<code>sudo cp /opt/netbox/contrib/*.service /etc/systemd/system/</code>  
<code>sudo systemctl daemon-reload</code>  
<code>sudo systemctl start netbox netbox-rq</code>  
<code>sudo systemctl enable netbox netbox-rq</code>  

Для настройки nginx были выполнены следующие команды:  
<code>sudo apt install -y nginx</code>  
<code>sudo cp /opt/netbox/contrib/nginx.conf /etc/nginx/sites-available/netbox</code>

После файл был отредактрирован, добавлен хост и удалены https server.  
<img src=https://user-images.githubusercontent.com/67152968/205461094-e6961900-e2b9-4287-846c-d62b109c06c7.png width=400>

Далее создадим символическую ссылку и перезапустим службу  
<code>sudo rm /etc/nginx/sites-enabled/default</code>  
<code>sudo ln -s /etc/nginx/sites-available/netbox /etc/nginx/sites-enabled/netbox</code>  
<code>sudo systemctl restart nginx</code>  

В итоге при переходе в браузере по ip открывается netbox, в который можно зайти под суперпользователем, который был создан ранее.

![image](https://user-images.githubusercontent.com/67152968/205461270-565abf8c-4b72-4671-851d-265f1258f380.png)


---
## 2. NetBox.

Была добавлена следующая минимально необходимая информация.

![image](https://user-images.githubusercontent.com/67152968/205461988-daa5638e-bfaf-4382-bf40-8f658eaee78e.png)

Для добавления ip-адресов была добавлен интерфейс для роутеров(во вкладке devices), позже ip адресам были предоставлены интерфейсы, после роутерам ip.

![image](https://user-images.githubusercontent.com/67152968/205507210-18f2d4fe-d9ac-4f9d-afcb-8a56ddf9c890.png)

После был скачен netbox_devices.csv файл с описанием девайсов.

## 3. Ansible Playbooks.

Файл netbox_devices.csv Был перенесен на сервер с anible.

Для начала создадим файл playbook1.yml и реализуем для проверки парсинг csv файла (сохраним название роутера в файл new_playbook1.yml, указав ID в качестве ключа)

<img src=https://user-images.githubusercontent.com/67152968/205510422-8b90d7c8-a72c-4680-963a-e7e184dfd00b.png width=400>

После playbook1 был выполнен и было записано сообщение с названием роутера: "router: mikrotik1"

<img src=https://user-images.githubusercontent.com/67152968/205510109-a0c7e85e-810a-4166-a219-cecc0168538f.png width=700>

Далее playbook1.yml был модифицирован для изменения имени устройства. В качестве нового имени для роутеров будет установлено название из netbox_devices.csv, которое спарсится в файл new_playbook1.yml.

<img src=https://user-images.githubusercontent.com/67152968/205510609-cd561af1-0871-46c8-a81b-d47ba4e6da3d.png width=400>

<img src=https://user-images.githubusercontent.com/67152968/205510886-366577b3-3f2b-4784-b701-f3b435b985ea.png width=600>

<img src=https://user-images.githubusercontent.com/67152968/205510854-0a45e7b0-926f-4509-94b6-4bbcb1265870.png width=400>

---

Для обратной задачи по изменению данных в netbox через ansible был сгенерирвоан api токен в интерфейсе netbox.  
Был создан следующий тестовый playbook  
<img src=https://user-images.githubusercontent.com/67152968/205512361-4c701c3a-3ef2-46d6-9aa6-f6a31f33a00d.png width=400>

После выполнения этого playbook в netbox был добавлен новый device

![image](https://user-images.githubusercontent.com/67152968/205512456-20dc0e9b-8ce0-4dfb-b229-0b09606c0d7f.png)

Был дописан playbook2.yml для сбора информации об имени устройства.

![image](https://user-images.githubusercontent.com/67152968/205513441-7eaadbbe-f555-4a80-96ff-0109111cf000.png)

После выполнения playbook на netbox в логах имеется информация о новом устройстве с именем из new_playbook.yml

![image](https://user-images.githubusercontent.com/67152968/205513334-df20bcbd-ba38-4271-8942-262901ef4ebf.png)

# Вывод
В результате работы была выполнена установка и настрйока netbox. Написаны ansible playbooks для взаимодействия ранее настроеных роутеров и netbox. Netbox показал себя как удобное средство для документирования сети.

## Схема связи
Со второй лабораторной она не сильно изменилась, так как netbox был установлен на той же машине, что и openvpn и ansible. Доступ к netbox обеспечиват nginx

![image](https://user-images.githubusercontent.com/67152968/206400381-d0c6744d-3572-482d-92a1-8736c375d01c.png)





