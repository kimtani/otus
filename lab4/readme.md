### Лабораторная работа. Настройка IPv6-адресов на сетевых устройствах

#### Топология

![](http://joxi.ru/823MaKXCaGj1Wr.jpg)

#### Таблица адресации

 | Устройство | Интерфейс | IPv6-адрес | Длина префикса |Шлюз по умолчанию|
 | ------------- |:------------------:| -----:|-----:|-----:|
 | R1 | G0/0/0 | 2001:db8:acad: a ::1 |64 |-|
 | | G0/0/1 | 2001:db8:acad:1::1 | 64 |-|
 |S1 | VLAN 1| 2001:db8:acad:1::b| 64|-|
 | PC-A | NIC | 2001:db8:acad:1::3 | 64 |fe80::1|
 | PC-B| NIC | 2001:db8:acad: a ::3 | 64 |fe80::1|

#### Задачи

##### Часть 1. Настройка топологии и конфигурации основных параметров маршрутизатора и коммутатора

##### Часть 2. Ручная настройка IPv6-адресов

##### Часть 3. Проверка сквозного соединения

#### Инструкции

##### Часть 1. Настройка топологии и конфигурации основных параметров маршрутизатора и коммутатора

##### Шаг 1. Настройте маршрутизатор

Назначьте имя хоста и настройте основные параметры маршрутизатора

``` Router>en
Router#conf t
Router(config)#hostname R1
R1(config)#enable secret class
R1(config)#banner motd #unauthorized access prohibited#
R1(config)#line console 0
R1(config-line)#login
% Login disabled on line 0, until 'password' is set
R1(config-line)#password cisco
R1(config-line)#exit
R1(config)#line vty 0 15
R1(config-line)#login
% Login disabled on line 2, until 'password' is set
% Login disabled on line 17, until 'password' is set
R1(config-line)#password cisco
R1(config-line)#exit
R1(config)#service password-encryption 
R1(config)#exit
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#
R1#conf t
Enter configuration commands, one per line. End with CNTL/Z.
R1(config)#line console 0
R1(config-line)#logging synchronous 
R1(config-line)#exit
R1(config)#inter g0/0/0
R1(config-if)#no sh
R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

R1(config-if)#ipv6 add 2001:db8:acad:a::1/64

R1(config)#inter g0/0/1
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#ipv6 ad
R1(config-if)#ipv6 address 2001:db8:acad:1::1/64
R1(config-if)#exit

```

##### Шаг 2. Настройте коммутатор

Назначьте имя хоста и настройте основные параметры утройства

```

```
##### Часть 2. Ручная настройка IPv6-адресов

##### Шаг 1. Назначьте IPv6-адреса интерфейсам Ethernet на R1 

##### Шаг 2. Активируйте IPv6 маршрутизацию на R1

##### Шаг 3. Назначьте IPv6-адреса интрефейсу управления (SVI) на R1


##### Часть 3. Проверка сквозного  подключения

##### Вопросы для повторения

1. Почему обоим интерфейсам Ethernet на R1 можно назначить один и тот локальный адрес канала
2. Какой идентификатор подсети в индивидуальном IPv6-адресе 
