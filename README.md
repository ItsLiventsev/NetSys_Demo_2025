## Стенд @pk8.mskobr по специальности 09.02.06 Сетевое и системное администрирование на 2024-2025 год

Базовый стенд представлен по сссылке - https://disk.yandex.ru/d/Qfry02DM_LYcGA (вложенный ахрив, открывать через 7-ZIP) (Стенд для добавления в VMware Player, вложенная виртуализация через ESXi). (в стенде могут быть изменения)

# Модуль 1 "Настройка сетевой инфраструктуры"

## Вводная информация по модулю 1

#### Топология сети

![alt text](image.png)

### Требования к ресурсам и гостевым ОС

![alt text](image-1.png)

### Таблица имен

![alt text](image-3.png)

## 1. Произведите базовую настройку устройств

Для базовой настройки ОС необходимо выдать имя хоста (hostname), IP-адреса на ВСЕ сетевые адаптеры, произвести обновление репозиториев, установить необходимые пакеты. (в задании указано IP-адрес должен быть из приватного диапазона, в случае, если сеть локальная, согласно RFC1918*)

### Выдача имени хоста (hostname)

mcedit /etc/hostname

![alt text](nz8pamLPe1ZrVSJCbvAXUFH-roquW76zof9K1VzL_nS6dDjS0qSwbdv-F8syBbeG3ioFo86eBwaWCiQ1y3dFTdo1.jpg)

Изменяем данные в файле на имя машины по заданию

![alt text](6CoifrrnG0jRMAXrQNzU9Lbo_dKR6hrf4UpTdEZiQUgOBWURnv3UlIWGNfxsMFgjcfUdrsB5rUirvIyG3ipZPB7_.jpg)

Сохраняем данные (Esc)

![alt text](image-5.png)

-------------------

### Настройка IP-адресов

Для проверки виртуальных сетевых адаптеров прописываем на ВМ команду - ip -c a

![alt text](image-6.png)

Если на ВМ не хватает сетевых адаптеров по схеме, то добавляем новые в настройках ВМ.

Правой кнопкой по окну ВМ или в списке ВМ и открываем Edit Setings

![alt text](image-7.png)

В меню выбираем Add network adapter

![alt text](image-8.png)

Далее проставляем сети в соответствии с заданием (схемой сети)

![alt text](image-9.png)

#### Корректные сетевые адаптеры для ISP

![alt text](image-10.png)

#### Корректные сетевые адаптеры для BR-RTR

![alt text](image-11.png)

#### Корректные сетевые адаптеры для BR-SRV

![alt text](image-12.png)

#### Корректные сетевые адаптеры для CLI

![alt text](image-13.png)

#### Корректные сетевые адаптеры для HQ-RTR

![alt text](image-14.png)

#### Корректные сетевые адаптеры для BR-DC

![alt text](image-15.png)

#### Корректные сетевые адаптеры для HQ-SRV

![alt text](image-16.png)

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

![alt text](image-18.png)

*названия адаптеров смотрим в ip -c a

**Директория для ens192 уже пресоздана на всех машинках

Копируем файл конфигурации сетевого адаптера ИЗ ENS192 в каждую директорию сетевого адаптера

![alt text](image-19.png)

#### Выдача IP-адреса по DHCP

![alt text](image-20.png)

![alt text](image-21.png)

![alt text](image-22.png)

![alt text](image-23.png)

#### Выдача статических адресов

![alt text](image-24.png)

![alt text](image-25.png)

#### Настройка основного шлюза для статики

![alt text](image-27.png)

![alt text](image-28.png)

Чтобы обновить данные - перезапускаем сервис сети

![alt text](image-26.png)

Видим по итогу, что сетевые адаптеры обновились и выдались адреса

![alt text](image-29.png)

Для примеения имени хоста - перезагружаем ВМ

![alt text](image-30.png)

![alt text](image-31.png)

-------------------

#### Пример настройки для HQ-SRV

![alt text](image-32.png)

#### Пример настройки для BR-SRV

![alt text](image-33.png)

#### Пример настройки для HQ-RTR

![alt text](image-34.png)

#### Пример настройки для CLI

![alt text](image-35.png)

#### Пример настройки для BR-R

![alt text](image-36.png)



![alt text](image-37.png)

--------------------

## 2. Настройка ISP

Настройте адресацию на интерфейсах: Интерфейс, подключенный к магистральному провайдеру, 
получает адрес по DHCP Настройте маршруты по умолчанию там, где это необходимо Интерфейс, к которому подключен HQ-RTR, подключен к сети 
172.16.4.0/28 Интерфейс, к которому подключен BR-RTR, подключен к сети 
172.16.5.0/28 На ISP настройте динамическую сетевую трансляцию в сторону HQ-RTR и BR-RTR для доступа к сети Интернет

apt-get update - обновление баз данных репозиториев

![alt text](image-38.png)

Установка пакета FRR - для работы маршрутизации - apt-get install frr

![alt text](image-39.png)


Добавляем сервис frr в автозагрузку - systemctl enable —now frr

![alt text](image-40.png)

*Для проверки работы пакета - systemctl status

![alt text](image-41.png)

### Конфигурация frr

Включаем OSPF и EIGRP

![alt text](image-42.png)

Ставим "yes" напротив ospfd и eigrpd - С МАЛЕНЬКОЙ БУКВЫ

![alt text](image-43.png)

Перезапускаем сервис для применения настройки

![alt text](image-44.png)

Заходим в терминал frr

![alt text](image-45.png)

*работаем как в обычной Cisco IOS

#### Конфигурация frr для ISP

![alt text](image-46.png)

#### Конфигурация frr для HQ-RTR

![alt text](image-47.png)

#### Конфигурация frr для BR-RTR

![alt text](image-48.png)

### Включаем IP Forwarding

mcedit /etc/net/sysctl.conf

Параметр net.ipv4.ip_forward=0 ставим на "1"

![alt text](image-49.png)

## 3. Создание локальных учетных записей

Создайте пользователя sshuser на серверах HQ-SRV и BR-SRV Пароль пользователя sshuser с паролем P@ssw0rd Идентификатор пользователя 1010 Пользователь sshuser должен иметь возможность запускать sudo без дополнительной аутентификации.

useradd - создание пользователя

Ключи к команде тут - https://edu-web.sferum.ru/doc709873984_680191273?hash=QEVZKP4LFyigZ1HPCNL1iOJ9D8rlzuxVD81zyXeBZ0X&dl=G4YDSOBXGM4TQNA%3A1725361377%3AwQXwRHJABAZB6lFf5UFyM5zhJQRdNUCKcEy7aPUVCac&from_module=vkmsg_desktop

Создаем пользователея с UID 1010

![alt text](image-50.png)

Изменяем пароль на пользователе (passwd sshuser)

![alt text](image-51.png)

*Проверяем какие группы пользователей могут пользоваться sudo без авторизации

![alt text](image-52.png)

Добавляем пользователя sshuser в группу

![alt text](image-53.png)

Проверяем что пользователь видится в двух группах

![alt text](image-54.png)

Даем пользователю права авторизации в sudo (root) без ввода пароля

![alt text](image-55.png)

Убираем комментарий с двух строчек

WHEEL_USERS ALL=(ALL:ALL) ALL

WHEEL_USERS ALL=(ALL;ALL) NOPASSWD: ALL

![alt text](image-56.png)

*Проверяем доступ к авторизации без пароля к пользователю sshuser

![alt text](image-57.png)

### Создайте пользователя net_admin на маршрутизаторах HQ-RTR и BR-RTR

Создаем пользователя

![alt text](image-58.png)

Изменяем пароль на P@$$word

![alt text](image-59.png)

Добавляем пользователя в группу

![alt text](image-60.png)

Изменяем параметры прав

![alt text](image-61.png)

Убираем комментарий с двух строчек

WHEEL_USERS ALL=(ALL:ALL) ALL

WHEEL_USERS ALL=(ALL;ALL) NOPASSWD: ALL

![alt text](image-62.png)

## 4. Настройте на интерфейсе HQ-RTR в сторону офиса HQ виртуальный коммутатор: (пока на доработке, VLANы видит, отображает, но пинги не идут) *МОЖЕТ ЛУЧШЕ ЧЕРЕЗ openvswitch

Для настройки будем использовать виртуальные интерфейсы. Создаем директории для подинтерфейсов .10 и .20

![alt text](image-72.png)

![alt text](image-73.png)

Настраиваем IP-адрес для подинтерфейса .10

![alt text](image-74.png)

![alt text](image-75.png)

Настраиваем файл options для подинтерфейса .10

![alt text](image-76.png)

По заданию сервер HQ-SRV должен находиться в ID VLAN 100

![alt text](image-77.png)

Настраиваем IP-адрес для подинтерфейса .20

![alt text](image-79.png)

По заданию клиент HQ-CLI в ID VLAN 200 

![alt text](image-80.png)

### Создайте подсеть управления с ID VLAN 999

![alt text](image-81.png)

Скопируем файл options из .10

![alt text](image-82.png)

![alt text](image-83.png)



## 5. Настройка безопасного удаленного доступа на серверах HQ-SRV и BR-SRV:

apt-get update - обновляем репозитории

apt-get install openssh-server

![alt text](image-63.png)

*При установке, могут возникнуть ошибки с работой пакета, в таком случае пишем systemctl daemon-reload

Дополнительно повторно проводим команду установки пакета

Добавляем сервис в автозагрузку systemctl enable —now sshd

![alt text](image-64.png)

#### Настройка файла конфигурации OpenSSH

![alt text](image-65.png)

Открываем файл, находим атрибут #port 22 - изменяем его на "port 2024" # - убираем

![alt text](image-66.png)

##### Ограничьте количество попыток входа до двух - меняем параметр с "6" на "2"

![alt text](image-67.png)

##### Настройте баннер «Authorized access only». Находим #no default banner path

Изменяем строчку, добавляя путь к файлу с данными по баннеру

![alt text](image-68.png)

Создаем файл с баннером

mcedit /etc/32admsbanner

В него прописываем баннер по заданию

![alt text](image-69.png)

##### Разрешите подключения только пользователю sshuser

Добавляем строчку AllowUsers и пишем имя пользователя с доступом

![alt text](image-70.png)

После перезагружаем сервис systemctl restart sshd

*Проверяем работу с другого устройства (HQ-RTR)

![alt text](image-71.png)

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

![alt text](image-84.png)

Добавляем в автозагрузку

![alt text](image-85.png)

Копируем файл example, создаем "чистовик" dhcpd.conf - он будет использоваться в качесвте конфигуратора DHCP сервера

![alt text](image-86.png)

Открываем вновь созданный файл

![alt text](image-87.png)

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

![alt text](image-89.png)

Сетевой адаптер в сторону сети HQ-NET

![alt text](image-90.png)

*Проверяем на CLI

IP-адрес был получен по DHCP с сервера

![alt text](image-91.png)

## 10. Настройка DNS для офисов HQ и BR.

## 11. Настройте часовой пояс на всех устройствах, согласно месту проведения экзамена.

По заданию просят только настроить часовой пояс на всех устройствах, менять время при этом не просят. Поэтому меняем пояс согласно месту проведения экзамена - Europe/Moscow

![image](https://github.com/user-attachments/assets/1716034c-a527-4c8c-b3b9-7b1f2446a365)

*если в задании поменяют регион на определенный, то посмотреть список регионов и городов можно тут

![image](https://github.com/user-attachments/assets/3f910af8-f188-4899-9145-aa9b6a8cbffa)

#### Пример заполнения таблицы адресов устройств

![alt text](image-4.png)

# Модуль 2 "Организация сетевого администрирования операционных систем"

# Модуль 3 "Эксплуатация объектов сетевой инфраструктуры"

## Модуль 4 "Вариативная часть" (в разработке)

# Данные для авториации в виртуальных машинах стенда

| Тип операционной системы | Логин      |  Пароль         |
| :---------------------:  | :--------: | :-------------: | 
| Eltex                    | admin      | P@ssw0rd        | 
| ALT_SRV                  | root       | P@ssw0rd        | 
| ALT_CLI                  | user       | P@ssw0rd        | 
| ASTRA_CLI                | root       | P@ssw0rd        |
