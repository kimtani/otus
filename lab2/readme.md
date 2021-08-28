## Лабораторная работа. Просмотр таблицы MAC-адресов коммутатора

#### Часть 1. Создание и настройка сети

#### Топология

![](http://joxi.ru/nAyD0MgUa47Qa2.jpg)

#### Таблица адресации

 | Устройство | Интерфейс | IP- адрес | Маска подсети |
 | ------------- |:------------------:| -----:|-----:|
 | S1 | VLAN1 | 192.168.1.11 | 255.255.255.0 |
 | S2 | VLAN2 | 192.168.1.12 | 255.255.255.0 |
 | PC-A | NIC | 192.168.1.1 | 255.255.255.0 |
 | PC-B | NIC | 192.168.1.2 | 255.255.255.0 |
 
#### Цели

##### **Часть 1. Создание и настройка сети.**

##### **Часть2. Изучение таблицы MAC-адресов.** 



##### Шаг 1. Подключите сеть в соответсвии с топологией.


##### Шаг 2. Настройте узлы ПК

##### Шаг 3. Выполните инициализацию и перезагрузку коммутаторов.

##### Шаг 4. Настройте базовые параметры каждого коммутатора.

a. Настройте имена устройств в соответствии с топологией.

b. Настройте IP-адреса, как указано в таблице адресации.

c. Назначьте **cisco**  в качестве паролей консоли и VTY.

d. Назначьте **class** в качестве пароля доступа к привелегированному режиму EXEC.

Настройки коммутатора S1

    Switch>en
    Switch#conf t
    Switch(config)#enable secret class
    Switch(config)#line console 0
    Switch(config-line)#login
    Switch(config-line)#password cisco
    Switch(config-line)#exit
    Switch(config)#line vty 0 15
    Switch(config-line)#login
    Switch(config-line)#password cisco
    Switch(config-line)#exit
    Switch(config)#hostname S1
    S1(config)#inter vlan1
    S1(config-if)#ip address 192.168.1.11 255.255.255.0
    S1(config-if)#no sh
  


d.

-при первоначальной настройке IP адрес не назначен

-при первоначальной настройке нет

-интерфейс выключен

e.

`Vlan1 is administratively down, line protocol is down`

`Hardware is CPU Interface, address is 0001.64b6.4806 (bia 0001.64b6.4806)`

`MTU 1500 bytes, BW 100000 Kbit, DLY 1000000 usec,`

`reliability 255/255, txload 1/255, rxload 1/255`

`Encapsulation ARPA, loopback not set`

`ARP type: ARPA, ARP Timeout 04:00:00`

`Last input 21:40:21, output never, output hang never`

g.

![](http://dl4.joxi.net/drive/2021/08/19/0050/1314/3282210/10/a5189f43e7.jpg)



h.

- интерефейс включен

- Интерфейс на коммутаторе при прямом подключении включается автоматически

- MAc-адрес 00e0.8fce.cd06 

- 100Mb/s, Full-duplex

i.

- Vlan 1 default

- все порты

- сеть Vlan 1 активна сразу при подключении

- сеть VLAN 1 виртуальная

j.

- 2960-lanbasek9-mz.150-2.SE4


#### Часть 2.
##### Шаг 1.
a.

    S1(config)#no ip domain-lookup  
    S1(config)#service password-encryption 
    S1(config)#enable secret class
    S1(config)#banner motd @Unautorised access is strictly prohibited@

