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

```

##### Шаг 2. Настройте коммутатор

Назначьте имя хоста и настройте основные параметры утройства

```Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#enable secret class
Switch(config)#line console 0
Switch(config-line)#login
% Login disabled on line 0, until 'password' is set
Switch(config-line)#password cisco
Switch(config-line)#exit
Switch(config)#line vty 0 15
Switch(config-line)#login
% Login disabled on line 1, until 'password' is set
% Login disabled on line 16, until 'password' is set
Switch(config-line)#password cisco
Switch(config-line)#exit
Switch(config)#service password-encryption 
Switch(config)#hostname S1
S1(config)#banner motd #Unauthorized access in strictly prohibited#

```
 -----
 
##### Часть 2. Ручная настройка IPv6-адресов

##### Шаг 1. Назначьте IPv6-адреса интерфейсам Ethernet на R1 

a. Назначьте  глобальные индивидуальные ipv6-адреса, указанные в таблице адресации обоим интерфейсам Ethernet на R1

```R1(config)#inter g0/0/0
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
b. Введите команду *show ipv6 interface brief*, чтобы проверить, назначен ли каждому интерфейсу корректный индивидуальный ipv6-адрес 

``` R1#sh ipv6 inter brief 
GigabitEthernet0/0/0       [up/up]
    FE80::201:42FF:FE87:B501
    2001:DB8:ACAD:A::1
GigabitEthernet0/0/1       [up/up]
    FE80::201:42FF:FE87:B502
    2001:DB8:ACAD:1::1
GigabitEthernet0/0/2       [administratively down/down]
    unassigned
Vlan1                      [administratively down/down]
    unassigned
```
c. Вручную введите локальные адреса канала накаждом интерфейсе Ethernet на R1

``` R1(config)#inter g0/0/0
R1(config-if)#ipv6 address fe80::1:1 link-local
R1(config-if)#no sh
R1(config-if)#exit
R1(config)#inter g0/0/1
R1(config-if)#ipv6 ad
R1(config-if)#ipv6 address fe80::1:1 lim
R1(config-if)#ipv6 address fe80::1:1 lin
R1(config-if)#ipv6 address fe80::1:1 link-local 
R1(config-if)#no sh
R1(config-if)#exit
```
d. Используйте выбранную команду, чтобы убедиться, что локальный адрес связи изменен на FE80::1:1

```R1#sh ipv6 inter brief 
GigabitEthernet0/0/0       [up/up]
    FE80::1:1
    2001:DB8:ACAD:A::1
GigabitEthernet0/0/1       [up/up]
    FE80::1:1
    2001:DB8:ACAD:1::1
GigabitEthernet0/0/2       [administratively down/down]
    unassigned
Vlan1                      [administratively down/down]
    unassigned
```
##### какие группы многоадресной рассылки назначены интерфейсу G0/0/0

##### Шаг 2. Активируйте IPv6 маршрутизацию на R1

a. В командной строке на PC-B введите команду *ipconfig*, чтобы получить данные ipv6-адреса, назначенного интерфейсу ПК

```C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::1
   IPv6 Address....................: 2001:DB8:ACAD:A::3
 ```

Назначен ли индивидуальный IPv6-адрес сетевой интерфейсной карте (NIC) на PC-B
 
  
b. Активируйте IPv6-маршрутизацию на R1  с помощью команды *IPv6 unicast routing*

    R1(config)#ipv6 unicast-routing

c. Введите командду *ipconfig* на PC-B

![](http://dl3.joxi.net/drive/2021/09/06/0050/1314/3282210/10/ec3ff4ea5d.jpg)


Почему PC-B получил глобальный префикс маршрутизации и идентификатор подсети, которые вы настроили на R1

?

##### Шаг 3. Назначьте IPv6-адреса интрефейсу управления (SVI) на S1

a. Назначьте IPv6-адрес для S1.Также назначьте этому интерфейсу локальный адрес канала

```
S1(config)#inter vlan 1
S1(config-if)#ipv6 add 2001:db8:acad:1::b/64
S1(config-if)#ipv6 ad fe80::1 link-local
```

b. Проверьте адрес назначения IPv6-адресов интерфейсу управления с помощью команды *show ipv6 interface vlan1*

![](http://joxi.ru/gmvD09NUdag9RA.jpg)

##### Шаг 4. Назначьте компьютерам статические IPv6-адреса

 PC-B 
 
![](http://joxi.ru/l2ZG5QoCRPYogA.jpg)

PC-A

![](http://joxi.ru/823MaKXCaLpW4r.jpg)



##### Часть 3. Проверка сквозного  подключения

![](http://joxi.ru/ZrJRa0oCbd7EXr.jpg)

![](http://joxi.ru/E2pDj9xU4EkW92.jpg)

##### Вопросы для повторения

1. Почему обоим интерфейсам Ethernet на R1 можно назначить один и тот локальный адрес канала

- Разные интерфейсы одного маршрутизатора принадлежат разным подсетям, поэтому один и тот же link-local адрес не имеет отношения к другому. 


2. Какой идентификатор подсети в индивидуальном IPv6-адресе 2001:db8:acad::aaaa:1234/64

  - Идентификатор подсети /64
