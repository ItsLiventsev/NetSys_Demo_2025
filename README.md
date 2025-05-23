## Стенд @pk8.mskobr по специальности 09.02.06 Сетевое и системное администрирование на 2024-2025 год

Базовый стенд представлен по сссылке - https://disk.yandex.ru/d/Qfry02DM_LYcGA (вложенный ахрив, открывать через 7-ZIP) (Стенд для добавления в VMware Player, вложенная виртуализация через ESXi). (в стенде могут быть изменения)

Для установки стенда через скрипт PVE (https://disk.yandex.ru/d/uT7Z3o0uaSBpjg), инструкция тут - https://itsliventsev.yonote.ru/share/548e46da-9cc5-41a8-9f77-0d4c7134ca73/doc/skript-dlya-avtorazvertyvaniya-stenda-de-2025-adm-na-pve-DT85xpRqXT

# Модуль 1 "Настройка сетевой инфраструктуры"

## Вводная информация по модулю 1

#### Топология сети

![alt text](screens/image.png)

### Требования к ресурсам и гостевым ОС

![alt text](screens/image-1.png)

### Таблица имен

![alt text](screens/image-3.png)

## 1. Произведите базовую настройку устройств

Для базовой настройки ОС необходимо выдать имя хоста (hostname), IP-адреса на ВСЕ сетевые адаптеры, произвести обновление репозиториев, установить необходимые пакеты. (в задании указано IP-адрес должен быть из приватного диапазона, в случае, если сеть локальная, согласно RFC1918*)

### Выдача имени хоста (hostname)

Пишем полное имя хоста - hq-rtr.au-team.irpo

mcedit /etc/hostname

![alt text](screens/nz8pamLPe1ZrVSJCbvAXUFH-roquW76zof9K1VzL_nS6dDjS0qSwbdv-F8syBbeG3ioFo86eBwaWCiQ1y3dFTdo1.jpg)

Изменяем данные в файле на имя машины по заданию

![alt text](screens/6CoifrrnG0jRMAXrQNzU9Lbo_dKR6hrf4UpTdEZiQUgOBWURnv3UlIWGNfxsMFgjcfUdrsB5rUirvIyG3ipZPB7_.jpg)

Сохраняем данные (Esc)

![image](https://github.com/user-attachments/assets/9c0eef7b-c4c2-45d4-93c0-3d24f79517e6)

###  Используйте полное доменное имя

domainname au-team.irpo

![image](https://github.com/user-attachments/assets/612ba191-b871-4020-9163-8a9a0532e199)

*Имена хостов и т.п.

![image](https://github.com/user-attachments/assets/b39c8276-5875-4f8d-9758-5e53fcc6cf94)

-------------------

### Настройка IP-адресов

Для проверки виртуальных сетевых адаптеров прописываем на ВМ команду - ip -c a

![alt text](screens/image-6.png)

Если на ВМ не хватает сетевых адаптеров по схеме, то добавляем новые в настройках ВМ.

Правой кнопкой по окну ВМ или в списке ВМ и открываем Edit Setings

![alt text](screens/image-7.png)

В меню выбираем Add network adapter

![alt text](screens/image-8.png)

Далее проставляем сети в соответствии с заданием (схемой сети)

![alt text](screens/image-9.png)

### Настройка DNS в сетевых адаптерах

*делаем настройку на всех машинах кроме ISP

![image](https://github.com/user-attachments/assets/1ed51a91-2b84-4b80-9a10-92861184a729)

![image](https://github.com/user-attachments/assets/734de9aa-e1e1-4c0d-ab85-38116edd3ebc)

#### Корректные сетевые адаптеры для ISP

![alt text](screens/image-10.png)

#### Корректные сетевые адаптеры для BR-RTR

![alt text](screens/image-11.png)

#### Корректные сетевые адаптеры для BR-SRV

![alt text](screens/image-12.png)

#### Корректные сетевые адаптеры для CLI

![alt text](screens/image-13.png)

#### Корректные сетевые адаптеры для HQ-RTR

![alt text](screens/image-14.png)

#### Корректные сетевые адаптеры для BR-DC

![alt text](screens/image-15.png)

#### Корректные сетевые адаптеры для HQ-SRV

![alt text](screens/image-16.png)

--------------------

*RFC1918 - меморандум Internet Engineering Task Force (IETF) о методах назначения частных IP-адресов в сетях TCP/IP.

| Блок адресов      | Макс.            |  Префиксы           |
| :-----------------| :--------:       | :-------------:     | 
| 10.0.0.0          | 10.255.255.255   | 10/8 prefix         | 
| 172.16.0.0        | 172.31.255.255   | 172.16/12 prefix    | 
| 192.168.0.0       | 192.168.255.255  | 192.168/16 prefix   |


#### Пример варианта IP-адресации (лучше также сделать в отдельно файлике у себя, чтобы не путаться)

![image](https://github.com/user-attachments/assets/33c33fc0-73b3-4070-88ca-f8b820e795d7)

#### Постановка адресации на ВМ без графического интерфейса

Создаем директории для новых адаптеров, по названию адаптеров*

![alt text](screens/image-18.png)

*названия адаптеров смотрим в ip -c a

**Директория для ens192 уже пресоздана на всех машинках

Копируем файл конфигурации сетевого адаптера ИЗ ENS192 в каждую директорию сетевого адаптера

![alt text](screens/image-19.png)

#### Выдача IP-адреса по DHCP

![alt text](screens/image-20.png)

![alt text](screens/image-21.png)

![alt text](screens/image-22.png)

![alt text](screens/image-23.png)

#### Выдача статических адресов

![alt text](screens/image-24.png)

![alt text](screens/image-25.png)

#### Настройка основного шлюза для статики (НА ВСЕХ УСТРОЙСТВАХ - BR-RTR, HQ-RTR на ISP, сеть HQ - на HR-RTR, сеть BR - на BR-RTR

![alt text](screens/image-27.png)

![alt text](screens/image-28.png)

Чтобы обновить данные - перезапускаем сервис сети

![alt text](screens/image-26.png)

Видим по итогу, что сетевые адаптеры обновились и выдались адреса

![alt text](screens/image-29.png)

Для примеения имени хоста - перезагружаем ВМ

![alt text](screens/image-30.png)

![alt text](screens/image-31.png)

-------------------

#### Пример настройки для HQ-SRV

![alt text](screens/image-32.png)

#### Пример настройки для BR-SRV

![alt text](screens/image-33.png)

#### Пример настройки для HQ-RTR

![alt text](screens/image-34.png)

#### Пример настройки для CLI

![alt text](screens/image-35.png)

#### Пример настройки для BR-R

![alt text](screens/image-36.png)



![alt text](screens/image-37.png)

--------------------

## 2. Настройка ISP

Настройте адресацию на интерфейсах: Интерфейс, подключенный к магистральному провайдеру, 
получает адрес по DHCP Настройте маршруты по умолчанию там, где это необходимо Интерфейс, к которому подключен HQ-RTR, подключен к сети 
172.16.4.0/28 Интерфейс, к которому подключен BR-RTR, подключен к сети 
172.16.5.0/28 На ISP настройте динамическую сетевую трансляцию в сторону HQ-RTR и BR-RTR для доступа к сети Интернет

apt-get update - обновление баз данных репозиториев

![alt text](screens/image-38.png)

Установка пакета FRR - для работы маршрутизации - apt-get install frr

![alt text](screens/image-39.png)


Добавляем сервис frr в автозагрузку - systemctl enable —now frr

![alt text](screens/image-40.png)

*Для проверки работы пакета - systemctl status

![alt text](screens/image-41.png)

### Конфигурация frr

Включаем OSPF и EIGRP

![alt text](screens/image-42.png)

Ставим "yes" напротив ospfd и eigrpd - С МАЛЕНЬКОЙ БУКВЫ

![alt text](screens/image-43.png)

Перезапускаем сервис для применения настройки

![alt text](screens/image-44.png)

Заходим в терминал frr

![alt text](screens/image-45.png)

*работаем как в обычной Cisco IOS

en - режим просмотра

conf t - режим конфигурации

router eigrp 1 - объявляем новый вариант динамической маршрутизации

network .......... - сети смотрим в скрнах, где конфигурация. (прописываем И ПОДСЕТЬ И МАСКУ, в качестве примера - "10.10.10.0/29)

network ..........

#### Конфигурация frr для ISP (ПИСАТЬ НА ИСПЕ, HQ-RTR И BR-RTR)

![image](https://github.com/user-attachments/assets/e7294794-b7ba-4dcb-95de-0df15e7e0205)

#### Конфигурация frr для HQ-RTR (10.10 УБИРАТЬ)

![image](https://github.com/user-attachments/assets/75c35f01-de94-4df7-88a2-98c5127f8172)

#### Конфигурация frr для BR-RTR (10.10 УБИРАТЬ)

![image](https://github.com/user-attachments/assets/6bfd8858-2589-4fda-95c9-ff1fe667e0a2)

#### Для проверки FRR

en

show ip eigrp topology - посмотреть "видимые" сети

![image](https://github.com/user-attachments/assets/a2eee370-b490-47af-9a05-36205dca2e23)

Должно быть видно все сети с ISP, *ВАЖНО, СЕТИ ОБНОВЛЯЮТСЯ ДО 10 МИНУТ*

### Включаем IP Forwarding

mcedit /etc/net/sysctl.conf

Параметр net.ipv4.ip_forward=0 ставим на "1"

Затем пишем - sysctl -p

![alt text](screens/image-49.png)

Затем пишем команду

sysctl net.ipv4.ip_forward=1

## 3. Создание локальных учетных записей

Создайте пользователя sshuser на серверах HQ-SRV и BR-SRV Пароль пользователя sshuser с паролем P@ssw0rd Идентификатор пользователя 1010 Пользователь sshuser должен иметь возможность запускать sudo без дополнительной аутентификации.

useradd - создание пользователя

Ключи к команде

![image](https://github.com/user-attachments/assets/cf060a75-3fcc-4262-94ae-e611fda0da30)

Создаем пользователея с UID 1010

![alt text](screens/image-50.png)

Изменяем пароль на пользователе (passwd sshuser)

![alt text](screens/image-51.png)

*Проверяем какие группы пользователей могут пользоваться sudo без авторизации

![alt text](screens/image-52.png)

Добавляем пользователя sshuser в группу

![alt text](screens/image-53.png)

Проверяем что пользователь видится в двух группах

![alt text](screens/image-54.png)

Даем пользователю права авторизации в sudo (root) без ввода пароля

![alt text](screens/image-55.png)

Убираем комментарий с двух строчек

WHEEL_USERS ALL=(ALL:ALL) ALL

WHEEL_USERS ALL=(ALL;ALL) NOPASSWD: ALL

![alt text](screens/image-56.png)

*Проверяем доступ к авторизации без пароля к пользователю sshuser

![alt text](screens/image-57.png)

### Создайте пользователя net_admin на маршрутизаторах HQ-RTR и BR-RTR

Создаем пользователя

![alt text](screens/image-58.png)

Изменяем пароль на P@$$word

![alt text](screens/image-59.png)

Добавляем пользователя в группу

![alt text](screens/image-60.png)

Изменяем параметры прав

![alt text](screens/image-61.png)

Убираем комментарий с двух строчек

WHEEL_USERS ALL=(ALL:ALL) ALL

WHEEL_USERS ALL=(ALL;ALL) NOPASSWD: ALL

![alt text](screens/image-62.png)

## 4. Настройте на интерфейсе HQ-RTR в сторону офиса HQ виртуальный коммутатор: (пока на доработке, VLANы видит, отображает, но пинги не идут) *МОЖЕТ ЛУЧШЕ ЧЕРЕЗ openvswitch

Для настройки будем использовать виртуальные интерфейсы. Создаем директории для подинтерфейсов .10 и .20

![alt text](screens/image-72.png)

![alt text](screens/image-73.png)

Настраиваем IP-адрес для подинтерфейса .10

![alt text](screens/image-74.png)

![alt text](screens/image-75.png)

Настраиваем файл options для подинтерфейса .10

![alt text](screens/image-76.png)

По заданию сервер HQ-SRV должен находиться в ID VLAN 100

![alt text](screens/image-77.png)

Настраиваем IP-адрес для подинтерфейса .20

![alt text](screens/image-79.png)

По заданию клиент HQ-CLI в ID VLAN 200 

![alt text](screens/image-80.png)

### Создайте подсеть управления с ID VLAN 999

![alt text](screens/image-81.png)

Скопируем файл options из .10

![alt text](screens/image-82.png)

![alt text](screens/image-83.png)



## 5. Настройка безопасного удаленного доступа на серверах HQ-SRV и BR-SRV:

apt-get update - обновляем репозитории

apt-get install openssh-server

![alt text](screens/image-63.png)

*При установке, могут возникнуть ошибки с работой пакета, в таком случае пишем systemctl daemon-reload

Дополнительно повторно проводим команду установки пакета

Добавляем сервис в автозагрузку systemctl enable —now sshd

![alt text](screens/image-64.png)

#### Настройка файла конфигурации OpenSSH

![alt text](screens/image-65.png)

Открываем файл, находим атрибут #port 22 - изменяем его на "port 2024" # - убираем

![alt text](screens/image-66.png)

##### Ограничьте количество попыток входа до двух - меняем параметр с "6" на "2"

![alt text](screens/image-67.png)

##### Настройте баннер «Authorized access only». Находим #no default banner path

Изменяем строчку, добавляя путь к файлу с данными по баннеру

![alt text](screens/image-68.png)

Создаем файл с баннером

mcedit /etc/32admsbanner

В него прописываем баннер по заданию

![alt text](screens/image-69.png)

##### Разрешите подключения только пользователю sshuser

Добавляем строчку AllowUsers и пишем имя пользователя с доступом

![alt text](screens/image-70.png)

После перезагружаем сервис systemctl restart sshd

*Проверяем работу с другого устройства (HQ-RTR)

![alt text](screens/image-71.png)

## 6. Между офисами HQ и BR необходимо сконфигурировать ip туннель

## 7. Обеспечьте динамическую маршрутизацию: ресурсы одного офиса должны быть доступны из другого офиса. Для обеспечения динамической маршрутизации используйте link state протокол на ваше усмотрение

Выполнение работы с ISP идентично на frr на BR-RTR и HQ-RTR

В качестве link state протокола можем использовать OSPF или EIGRP, в примере предоставлен вариант настройки через протокол EIGRP.

Конфигурации в пункте 2 - https://github.com/ItsLiventsev/NetSys_Demo_2025?tab=readme-ov-file#%D0%BA%D0%BE%D0%BD%D1%84%D0%B8%D0%B3%D1%83%D1%80%D0%B0%D1%86%D0%B8%D1%8F-frr

## 8. Настройка динамической трансляции адресов.

## 9. Настройка протокола динамической конфигурации хостов.

Настройте нужную подсеть .Для офиса HQ в качестве сервера DHCP выступает маршрутизатор HQ-RTR. Клиентом является машина HQ-CLI. Исключите из выдачи адрес маршрутизатора. Адрес шлюза по умолчанию – адрес маршрутизатора HQ-RTR. Адрес DNS-сервера для машины HQ-CLI – адрес сервера HQ-SRV. DNS-суффикс для офисов HQ – au-team.irpo. Сведения о настройке протокола занесите в отчёт

DHCP — сетевой протокол, позволяющий сетевым устройствам автоматически получать IP-адрес и другие параметры, необходимые для работы в сети TCP/IP.

Устанавливаем пакет dhcp-server

apt-get install dhcp-server

![alt text](screens/image-84.png)

Добавляем в автозагрузку

![alt text](screens/image-85.png)

Копируем файл example, создаем "чистовик" dhcpd.conf - он будет использоваться в качесвте конфигуратора DHCP сервера

![alt text](screens/image-86.png)

Открываем вновь созданный файл

![alt text](screens/image-87.png)

Производим настройку*

![image](https://github.com/user-attachments/assets/26ab99eb-2523-4258-b6ce-1163c9c3d111)

*Некоторые общие параметры сервера DHCP:

subnet— Параметр объявляет подсеть (в нашем случае 192.168.38.0 с маской 255.255.255.0)

range  – Диапазон выдаваемых адресов

option subnet-mask – Маска сети.

option broadcast-address – Широковещательный адрес.

domain-name-servers – Адреса серверов DNS.

option domain-name  – Доменное имя.

option routers – Определяет IP-адрес вашего шлюза или точки выхода в сеть.

Чтобы задать время аренды по умолчанию и максимальное время аренды

default-lease-time 600;

max-lease-time 7200;

Для резервирования адреса добавляем строки "host"

host SERVER {
  hardware ethernet 08:60:6e:d6:5e:ff;
  fixed-address 192.168.38.5;}
}

Выставляем сетевой адаптер, который будет работать на раздачу DHCP

![alt text](screens/image-89.png)

Сетевой адаптер в сторону сети HQ-NET

![alt text](screens/image-90.png)

*Проверяем на CLI

IP-адрес был получен по DHCP с сервера

![alt text](screens/image-91.png)

## 10. Настройка DNS для офисов HQ и BR.

## 11. Настройте часовой пояс на всех устройствах, согласно месту проведения экзамена.

По заданию просят только настроить часовой пояс на всех устройствах, менять время при этом не просят. Поэтому меняем пояс согласно месту проведения экзамена - Europe/Moscow

![image](https://github.com/user-attachments/assets/1716034c-a527-4c8c-b3b9-7b1f2446a365)

*если в задании поменяют регион на определенный, то посмотреть список регионов и городов можно тут

![image](https://github.com/user-attachments/assets/3f910af8-f188-4899-9145-aa9b6a8cbffa)

#### Пример заполнения таблицы адресов устройств

![alt text](screens/image-4.png)

# Модуль 2 "Организация сетевого администрирования операционных систем"

## 1. Настройте доменный контроллер Samba на машине BR-SRV. Создайте 5 пользователей для офиса HQ: имена пользователей формата user№.hq. Создайте группу hq, введите в эту группу созданных пользователей. Введите в домен машину HQ-CLI. Пользователи группы hq имеют право аутентифицироваться на клиентском ПК. Пользователи группы hq должны иметь возможность повышать привилегии для выполнения ограниченного набора команд: cat, grep, id. Запускать другие команды с повышенными привилегиями пользователи группы не имеют права. Выполните импорт пользователей из файла users.csv. Файл будет располагаться на виртуальной машине BR-SRV в папке /opt (ДОЛГО И НУДНО, ПЛЮС ВЫ НИКОГДА НЕ УСПЕВАЕТЕ, ПОЭТОМУ ПОКА ПРОПУСКАМ)

## 2. Сконфигурируйте файловое хранилище: • При помощи трёх дополнительных дисков, размером 1Гб каждый, на HQ-SRV сконфигурируйте дисковый массив уровня 5 • Имя устройства – md0, конфигурация массива размещается в файле /etc/mdadm.conf • Обеспечьте автоматическое монтирование в папку /raid5 • Создайте раздел, отформатируйте раздел, в качестве файловой системы используйте ext4

### Добавляем виртуальные жесткие диски в ВМ HQ-SRV

Открываем настройки ВМ и добавляем три виртуальных диска по 1 гб каждый

![Снимок экрана 2024-09-10 152915](https://github.com/user-attachments/assets/90d9569a-00b4-4141-a10b-f456ff293df2)

![Снимок экрана 2024-09-10 152942](https://github.com/user-attachments/assets/41a60f8b-212d-4b13-bb42-6b50116b58c5)

![Снимок экрана 2024-09-10 153000](https://github.com/user-attachments/assets/21d34806-3e6f-4794-bcb4-4502359c1c17)

### Работа с parted

*название разделов и название дисков может отличаться

Для работы с дисками используем утилиту - parted

![Снимок экрана 2024-09-10 153207](https://github.com/user-attachments/assets/28c07a21-2c43-4487-ae93-f9fccf689654)

Чтобы посмотреть "физические" (виртуальные) диски пишем print devices

![Снимок экрана 2024-09-10 153258](https://github.com/user-attachments/assets/686e0724-23e4-44e6-95ff-691dcf86cf2c)

ДИСК /dev/sda НЕ ТРОГАТЬ - ЭТО ДИСК С ОПЕРАЦИОННОЙ СИСТЕМОЙ

Диски sdb, sdc, sdd - наши вновь созданные виртуальные диски для создания RAID 5

#### *Немного теории по parted

Если нужно настроить конкретный диск, то его сначала надо выбрать - select *диск*

![Снимок экрана 2024-09-10 153417](https://github.com/user-attachments/assets/4f8955ed-a919-4f4e-b65f-1fd588cdad78)

Для создания таблицы разделов используется команда mktable *тип таблицы*

![Снимок экрана 2024-09-10 154245](https://github.com/user-attachments/assets/8d1a8e21-8a53-4ee3-a50f-5dfa676d0c08)

Для вывода информации по определенному диску - после выбора диска используйте команду - print

Для создания нового раздела (логического диска) используем команду mkpart (в атрибутах можно указывать размеры в битах, мб или использовать проценты

![Снимок экрана 2024-09-10 154250](https://github.com/user-attachments/assets/56e972fb-a83b-4e70-849d-1f77f6ee3178)

### Работа с программным контроллером RAID. Создание RAID 5

*название разделов и название дисков может отличаться

Для конфигурации RAID используется утилита mdadm

Для создания RAID используем следующую команду:

mdadm --create --level=5 --raid-devices=3 /dev/md/md0 /dev/sdb /dev/sdc /dev/sdd

![Снимок экрана 2024-09-10 155202](https://github.com/user-attachments/assets/2c4d1055-8c9d-462e-8997-6ead109482e0)

где:

--create - создать

--verbose - выводить подробную информацию при работе утилиты mdadm

/dev/md/md0 — имя блочного устройства RAID, которое появится после сборки массива

--level=5 — уровень RAID массива (1,2,5,10)

--raid-devices=3 — количество дисков, включаемых в массив

/dev/sdb /dev/sdc /dev/sdd — имена дисков, включаемых в массив

### Отформатируйте раздел, в качестве файловой системы используйте ext4

*название разделов и название дисков может отличаться

![Снимок экрана 2024-09-10 160209](https://github.com/user-attachments/assets/633646be-2b7b-4b8d-97f7-3af1189760c8)

### Обеспечьте автоматическое монтирование в папку /raid5

Для конфигурации автоматического монтирования в систему - используется файл /etc/fstab

![Снимок экрана 2024-09-11 083726](https://github.com/user-attachments/assets/4e364cd9-661f-4516-9654-f83bba78db72)

В файле необходимо создать новую запись, в котором указать атрибуты монтирования

![image](https://github.com/user-attachments/assets/703926cb-e3b8-4aa3-a999-b06b15288817)

где:

* filesystem. Физическое место размещения файловой системы, по которому определяется конкретный раздел или устройство хранения для монтирования. 

* dir. Точка монтирования, куда монтируется корень файловой системы. 
*type.  Тип файловой системы. Поддерживается множество типов. 

*options.  Параметры монтирования файловой системы. 

*dump.  Используется утилитой dump для определения того, нужно ли создать резервную копию данных в файловой системе. Возможные значения: 0 или 1. 

*pass. Используется программой fsck для определения того, нужно ли проверять целостность файловой системы. Возможные значения: 0, 1 или 2. 

### Настройте сервер сетевой файловой системы(nfs), в качестве папки общего доступа выберите /raid5/nfs, доступ для чтения и записи для всей сети в сторону HQ-CLI

Обновляем репозитории, устанавливаем пакет nfs-server

![Снимок экрана 2024-09-12 115050](https://github.com/user-attachments/assets/7f818660-51a4-467b-8a4a-b1ed81162eb6)

![Снимок экрана 2024-09-12 115103](https://github.com/user-attachments/assets/4ad9d7c7-0df5-4011-9d46-2fa16b09144d)

Добавляем nfs-server в автозагрузку

![Снимок экрана 2024-09-12 115449](https://github.com/user-attachments/assets/20bf36f4-96a7-4c26-876f-ef7e01c2708a)

Создаем папку nfs на нашем RAID-массиве (обратите внимание, RAID должен быть монтирован в папку /raid5)

![Снимок экрана 2024-09-12 115551](https://github.com/user-attachments/assets/7e2b05f6-56a8-4ac1-ad78-e960a0a526f3)

Производим конфигурацию NFS

![Снимок экрана 2024-09-12 115657](https://github.com/user-attachments/assets/b7b967ae-e26b-4c6f-a9a3-28e9ce0b9551)

/raid5/nfs 192.168.1.0/29(rw,subtree_check,no_root_squash)

![ERj4Y6QDhV7K4vzp_xL1Vd5bIO0CVOHA2xPGtlBebca6K4vcnYSk1iSRrlrMKUeMJYqx_4YNvOU7QPzCqpi3ljQv (1)](https://github.com/user-attachments/assets/bf07304b-9017-4610-bf6a-94dc4baf14ed)

#### На HQ-CLI настройте автомонтирование в папку /mnt/nfs (*ДОДЕЛАТЬ МОНТИРОВАНИЕ)

*Проверяем для теста соединения и монтирование папки вообще

![lNcnwbXPl-m2m4ITkMe409lXfTmYDOSyF1yBuT8CqKPHi5ZbFdlNU0PpUgxlphwrjBunFOHBN2V_ziWzF3B_kllH](https://github.com/user-attachments/assets/ce0a375a-90ca-4ed7-b6ef-ded79112d998)

Пробуем создать файлик

![image](https://github.com/user-attachments/assets/1d5cf5c5-9f99-4a30-9ac6-46ddb80efded)

Видим, что файлик есть

![image](https://github.com/user-attachments/assets/e6157fdb-eb13-4f71-88dc-be5fe0b7c01e)

Для автоматического монтирования на клиенте используем fstab (*ДОДЕЛАТЬ МОНТИРОВАНИЕ)

Добавляем новую запись

![image](https://github.com/user-attachments/assets/04776eb5-5c7f-4c5a-9fd2-f7ece6a606dd)

После перезагрузки, автомонтирование работает
##### Основные параметры сервера отметьте в отчёте

### 5. Развертывание приложений в Docker на сервере BR-SRV. (взято у @abdurrah1m🥲)

• Создайте в домашней директории пользователя файл wiki.yml для приложения MediaWiki.
• Средствами docker compose должен создаваться стек контейнеров с приложением MediaWiki и базой данных.
• Используйте два сервиса
• Основной контейнер MediaWiki должен называться wiki и использовать образ mediawiki
• Файл LocalSettings.php с корректными настройками должен находиться в домашней папке пользователя и автоматически монтироваться в образ.
• Контейнер с базой данных должен называться mariadb и использовать образ mariadb.
• Разверните
• Он должен создавать базу с названием mediawiki, доступную по стандартному порту, пользователя wiki с паролем WikiP@ssw0rd должен иметь права доступа к этой базе данных
• MediaWiki должна быть доступна извне через порт 8080.

Установка Docker и Docker-compose:
```
apt-get update && apt-get install -y docker-engine
apt-get install -y docker-compose
```
Автозагрузка `Docker`:
```
systemctl enable --now docker
```
Привязка пользователя к `Docker`:
```
usermod user -aG docker
```
Переходим к домашней директории пользователя:
```
cd /home/user
```
Создаём файл wiki.yml:
```
touch wiki.yml
```
```yml
version: '3'
services:
  wiki:
    image: mediawiki
    restart: always
    ports:
      - 8080:80
    links:
      - database
    container_name: wiki
    volumes:
      - images:/var/www/html/images
# Сначала устанавливаем вручную до конца, потом убираем комментарий
#      - ./LocalSettings.php:/var/www/html/LocalSettings.php
  database:
    image: mariadb
    container_name: mariadb
    restart: always
    environment:
      MYSQL_DATABASE: mediawiki
      MYSQL_USER: wiki
      MYSQL_PASSWORD: WikiP@ssw0rd
      MYSQL_RANDOM_ROOT_PASSWORD: 'yes'
      TZ: Asia/Yekaterinburg
    volumes:
      - db:/var/lib/mysql
volumes:
  images:
  db:
```
Запускаем контейнеры:
```
docker compose -f wiki.yml up -d
```
Переходим по `<ip-сервера>:8080` и должно появиться - 'Please set up the wiki first'

![image](https://github.com/user-attachments/assets/7f31fa74-9b7d-448b-b151-60fea4101b68)

Для того, чтобы узнать хост базы данных:
```
docker exec -it mariadb bash
```
```
hostname -i
```
Вывод
```
172.18.0.2
```

![image](https://github.com/user-attachments/assets/fad6ff4f-29a2-42ce-9895-7bc551386a0b)

Принимаем условия `Далее`

![image](https://github.com/user-attachments/assets/d5a8a841-be14-4e63-a779-bbdcda6ff6ea)

![image](https://github.com/user-attachments/assets/b1b2d5ab-a628-4e0f-9129-21e50a2bc2b2)

![image](https://github.com/user-attachments/assets/d82f8c7c-ba3a-446d-b1e2-3b58328f8fee)

![image](https://github.com/user-attachments/assets/c68ab5ec-5082-4912-a008-85ad0d5ed584)

![image](https://github.com/user-attachments/assets/0d147398-8dea-4c79-a73d-3dceb421d349)

![image](https://github.com/user-attachments/assets/f5546510-c41f-46e0-a73f-893596047481)

![image](https://github.com/user-attachments/assets/04b4e49e-5bba-4cb3-9f66-40bf299be64e)

![image](https://github.com/user-attachments/assets/7ebd8118-bd70-4a41-9b82-b5a52ae04a67)


# Модуль 3 "Эксплуатация объектов сетевой инфраструктуры"

## 1. Настройте доменный контроллер Samba на машине BR-SRV (пока нет)

## 2. Сконфигурируйте файловое хранилище: (пока нет)

## 3. Настройте службу сетевого времени на базе сервиса chrony

1. Устанавливаем chrony на HQ-RTR командой:

apt-get install chrony

2. Далее редактируем конфигурационный файл /etc/chrony/chrony.conf

mcedit /etc/chrony/chrony.conf

#server ntp4.uniiftri.ru iburst <- ПОДОБНЫЕ ЗАПИСИ КОММЕНТИРУЕМ!!!
#pool 2.debian.pool.ntp.org iburst

/// ДОПИСЫВАЕМ ВСЁ ЧТО СНИЗУ ///

server 127.0.0.1 iburst prefer

local stratum 5

(КАК ПРИМЕР, СЕТИ МЕНЯЙТЕ НА СВОИ)

allow 172.16.0.0/30

allow 192.168.100.0/26

allow 192.168.200.0/28

allow 192.168.0.0/27

[что к чему]

server - машина выступающая на роль сервера chrony;

iburst - отправка нескольких пакетов (для точности);

perfer - указывает на предпочитаемый сервер;

local stratum 5 - установка 5 уровня на локальный сервер;

allow - устройства с каких подсетей имеют возможность синхронизироваться с сервером;


3. После установки, перезагружаем сервис и добавляем в автозагрузку:

systemctl restart chrony

systemctl enable --now  chrony

## 4. Сконфигурируйте ansible на сервере BR-SRV (пока нет)

## 5. Развертывание приложений в Docker на сервере BR-SRV. (пока нет)

## 6. На маршрутизаторах сконфигурируйте статическую трансляцию портов (пока нет)

## 7. Запустите сервис moodle на сервере HQ-SRV: (пока нет)

## 8. Настройте веб-сервер nginx как обратный прокси-сервер на HQ-RTR (пока нет)

## 9. Удобным способом установите приложение Яндекс Браузере для организаций на HQ-CLI

Если есть встроенный браузер - скачать Яндекс с его помощью

Если нет - установка при помощи команды:

apt-get install yandex-browser-stable



# Модуль 4 "Вариативная часть" (ответ пока от GPT, не смотрел)

**Модуль 1: Организация доступа в Интернет и блокировка ресурсов**

## Настройка DHCP на интерфейсе `ens192`

    Подключение проводим как обычно через файлы ens192: optionns - ставим параметр на dhcp

2.  **Блокировка доступа к `https://vk.com` и `https://www.youtube.com` через файл `/etc/hosts`:**

    *   **Редактирование файла `/etc/hosts` с помощью `mcedit`:**
        ```bash
        mcedit /etc/hosts
        ```
    *   **Добавление записей:**
        ```
        127.0.0.1       vk.com
        127.0.0.1       www.vk.com
        127.0.0.1       youtube.com
        127.0.0.1       www.youtube.com
        ```
    *   **Сохранение изменений:**  `F2` (сохранить), `Enter`, `F10` (выйти).
    *   **Очистка DNS-кэша (может потребоваться):**
        ```bash
        systemctl restart nscd
        ```
    *   **Проверка блокировки:**  Открытие сайтов в браузере.

3.  **Создание "особого" файла с информацией:** (сначала смотрим задание модуля 3, где надо создать пользователя, после его настройки уже делаем скрипт)

    *   **Создание скрипта:**
        ```bash
        mcedit /home/resu/info.sh
        ```
    *   **Добавление кода скрипта:**
        ```bash
        #!/bin/bash

        echo "Дата и время: $(date)"
        echo "Активные пользователи:"
        who
        echo "Мой IP-адрес:"
        ip addr show ens192 | grep "inet " | awk '{print $2}' | cut -d'/' -f1
        echo "Свободное место на диске:"
        df -h
        ```
    *   **Сохранение скрипта:** `F2` (сохранить), `Enter`, `F10` (выйти).
    *   **Сделать скрипт исполняемым:**
        ```bash
        chmod +x /home/resu/info.sh
        ```
    *   **Проверка работы скрипта:**
        ```bash
        /home/resu/info.sh
        ```

**Модуль 2: Автоматическая настройка интерфейса (DHCP) - выполнено в Модуле 1**

**Модуль 3: Эксплуатация объектов сетевой инфраструктуры**

1.  **Постоянная установка операционной системы:** (проблемы нет, она улслоно решена сразу)

    *   **Проблема:** Загрузка с установочного носителя вместо жесткого диска.
    *   **Решение:**
        *   Вход в BIOS/UEFI (Delete, F2, F12, Esc).
        *   Настройка порядка загрузки (Boot Order).
        *   Установка жесткого диска (HDD) в качестве первого устройства загрузки.
        *   Сохранение изменений и выход (F10, Save & Exit).
        *   Проверка загрузки.

2.  **Постоянный выход из системы (автоматический выход должен быть в 20:30):** (ПРОБЛЕМУ НАДО РЕШИТЬ)

    *   **Проблема:** Настроен автоматический выход из системы слишком часто.
    *   **Решение:**
        *   Проверка настроек энергосбережения (Power Management).
        *   Проверка настроек блокировки экрана (Screen Lock).
        *   Настройка запланированной задачи (cron):
            ```bash
            crontab -e
            ```
            Добавление строки:
            ```
            30 20 * * * loginctl terminate-user resu
            ```
            Сохранение изменений.

3.  **Неверный логин/пароль для пользователя `resu` (пользователя изначально нет в системе):**

    *   **Создание пользователя `resu`:**
        ```bash
        useradd resu
        ```
    *   **Установка пароля для пользователя `resu`:**
        ```bash
        passwd resu
        ```
        Ввод пароля `P@$$word` (и подтверждение).

4.  **Отсутствие доступа пользователя `resu` к `/mnt/work_disk`:**

    *   **Создание точки монтирования (если ее не существует):**
        ```bash
        mkdir -p /mnt/work_disk
        ```
    *   **Определение UUID диска:**
        ```bash
        blkid
        ```
    *   **Редактирование `/etc/fstab` с помощью `mcedit`:**
        ```bash
        mcedit /etc/fstab
        ```
        Добавление строки (замените UUID):
        ```
        UUID="a1b2c3d4-e5f6-7890-1234-567890abcdef" /mnt/work_disk ext4 defaults 0 0
        ```
    *   **Изменение владельца точки монтирования:**
        ```bash
        chown resu:resu /mnt/work_disk
        ```
    *   **Монтирование диска:**
        ```bash
        mount /mnt/work_disk
        ```
    *   **Проверка доступа:** Залогиниться под `resu` и проверить доступ к `/mnt/work_disk`.

5.  **Отсутствие прав на редактирование файлов в папке `/var` у пользователя `resu`:**

    *   **Создание группы для управления доступом к `/var` (рекомендуется):**
        ```bash
        groupadd var_editors
        ```
    *   **Добавление пользователя `resu` в группу:**
        ```bash
        usermod -a -G var_editors resu
        ```
    *   **Изменение владельца группы для каталога `/var`:**
        ```bash
        chgrp var_editors /var
        ```
    *   **Предоставление прав на запись группе `var_editors` для каталога `/var`:**
        ```bash
        chmod g+w /var
        ```
    *  **Сделайте группу владельцем всех новых файлов.**
       ```bash
       chmod g+s /var
       ```
    *   **Проверка доступа:** Залогиниться под `resu` и попробовать создать/редактировать файл в `/var`.

# Данные для авториации в виртуальных машинах стенда

| Тип операционной системы | Логин      |  Пароль         |
| :---------------------:  | :--------: | :-------------: | 
| Eltex                    | admin      | P@ssw0rd        | 
| ALT_SRV                  | root       | P@ssw0rd        | 
| ALT_CLI                  | user       | P@ssw0rd        | 
| ASTRA_CLI                | root       | P@ssw0rd        |
