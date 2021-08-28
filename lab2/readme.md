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

##### Часть 1. **Создание и настройка сети.**

##### Часть2. **Изучение таблицы MAC-адресов.** 

------

##### Часть 1. **Создание и настройка сети.**

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
  
![](http://joxi.ru/D2Pkx0oCBv0yXm.jpg)

Настройки коммутатора S2

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
    Switch(config)#hostname S2
    S2(config)#inter vlan1
    S2(config-if)#ip address 192.168.1.12 255.255.255.0
    S2(config-if)#no sh


![](http://joxi.ru/82Q1kNluVKEJO2.jpg)

-----


##### Часть2. **Изучение таблицы MAC-адресов.** 

##### Шаг 1. Запишите MAC-адреса сетевых устройств

a. PC-A

![](http://dl4.joxi.net/drive/2021/08/28/0050/1314/3282210/10/3f58576f46.jpg)
   
   PC-B

![](http://dl4.joxi.net/drive/2021/08/28/0050/1314/3282210/10/c41f78b089.jpg)

b. S1

![](http://dl4.joxi.net/drive/2021/08/28/0050/1314/3282210/10/3e1b9df63d.jpg)

   S2
   
![](http://dl3.joxi.net/drive/2021/08/28/0050/1314/3282210/10/d2325e0247.jpg)   

##### Шаг 2. Просмотрите таблицу MAC-адресов коммутатора.

Таблица MAC-адресов **до** тестирования сетевой связи с помощью эхо-запросов

```User Access Verification

Password: 

S2>en
Password: 
S2#sh mac-address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0001.6393.b901    DYNAMIC     Fa0/1
   
```   


Таблица MAC-адресов **после** тестирования сетевой связи с помощь. эхо-запросов

```  S2>en
Password: 
S2#ping 192.168.1.11

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.11, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/2/10 ms

S2#sh mac-ad
S2#sh mac-address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0001.6393.b901    DYNAMIC     Fa0/1
   1    0030.f275.2248    DYNAMIC     Fa0/1
   
```  
В таблице MAC-адресов записан только один MAC-адрес - 0001.6393.b901 . Сопоставлен с портом Fa0/1.
Данный MAC-адрес принадлежит коммутатору S1.

##### Шаг 3.

```S2#clear mac-address-table dynamic 
S2#show mac-address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0001.6393.b901    DYNAMIC     Fa0/1

S2#show mac-address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    0001.6393.b901    DYNAMIC     Fa0/1
```


##### Шаг 4.  С компьютера PC-B отправьте эхо-запросы устройствам сети и просмотрите таблицу MAC-адресов коммутатора.

a.

```C:\>  arp -a
No ARP Entries Found
```

b.

![](http://joxi.ru/L21qKdOUzvP6B2.jpg)


