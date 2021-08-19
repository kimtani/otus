## Лабораторная работа. Базовая настройка коммутатора

#### Часть 1. Создание сети и проверка настроек коммутатора по умолчанию
##### Шаг 1.
a. Подсоедините консольный кабель, как показано в топологии.

![](https://github.com/miscakes/otus/blob/main/images/Cisco%20Packet%20Tracer.jpg)

b.Почему нужно использовать консольное подключение при первоначальной настройке коммутатора? Почему нельзя подключиться к коммутатору через Telnet или SSH?
- При первоначальной настройке необходимо подключение непосредственно через консольный порт, так как у коммутатора нет устройства отображения и управление осуществляется через программу эмуляции терминала
- Управление через консоль доступно сразу, а вот подключение через Telnet необходимо настраивать. Для этого надо задать IP адрес устройству.

##### Шаг 2.
a.

![](https://github.com/miscakes/otus/blob/main/images/S1.jpg)

![](https://github.com/miscakes/otus/blob/main/images/S2.jpg)

b.
- 24 Интерфейса FastEthernet
- 2 интерфейса GigabitEthernet
- диапазон vty от 0 до 15

c.

S1#sh startup-config 

startup-config is not present

- коммутатор еще не настроен

- 

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

b.

    S1(config)#inter vlan 1
    S1(config-if)#ip ad 192.168.1.200 255.255.0.0
    S1(config-if)#
    
c.

    S1(config)#line console 0
    S1(config-line)#password cisco
    S1(config-line)#login
    S1(config-line)#
    S1(config-line)#logging synchronous 

  
d.

    S1(config)#line vty 0 4
    S1(config-line)#password cisco2  
    S1(config-line)#login

Команда login инструктирует коммутатор о том, что нужно запрашивать текстовый пароль

##### Шаг 2.

![](http://joxi.ru/V2VJNgot8MYxQ2.jpg)


#### Часть 3. Проверка сетевых подключений
##### Шаг 1.

a.

![](http://joxi.ru/5mdVO36iaPpepA.jpg)




![](https://github.com/miscakes/otus/blob/main/images/a3.jpg)

b. Проверьте параметры VLAN 1

- BW 100000 Kbit

- Vlan 1 is down

- Line protocol is down

##### Шаг 2.

a.

![](http://joxi.ru/bmoeq9XF7kX7lA.jpg)


##### Шаг 3.


d.

    S1#copy running-config startup-config 
    Destination filename [startup-config]? 
    Building configuration...
    [OK]
    S1#
    S1#exit



#### Вопросы для повторения
1. Пароль VTY нужен для безопасного удаленного подключения к коммутатору
2. Ввести команду password encryption в режиме конфигурации виртуальных линий

