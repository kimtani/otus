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

##### Часть 2. **Изучение таблицы MAC-адресов коммутатора.** 

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


##### Часть 2. **Изучение таблицы MAC-адресов коммутатора.** 

##### Шаг 1. Запишите MAC-адреса сетевых устройств

a. PC-A

![](http://dl3.joxi.net/drive/2021/09/12/0050/1314/3282210/10/5a98a3f823.jpg)
   
   PC-B

![](http://dl3.joxi.net/drive/2021/09/12/0050/1314/3282210/10/de69454b22.jpg)

Физические адреса адаптера Ethernet.

 - MAC-адрес компьютера PC-A - 000C.8502.3ED2
 - MAC-адрес компьютера PC-B - 0060.4743.AA40

b. Подключитесь к коммутаторам  S1 и S2 через консоль и введите команду *show interface f0/1*

S1


```S1#show inter f0/1
FastEthernet0/1 is up, line protocol is up (connected)
  Hardware is Lance, address is 00d0.ff78.4301 (bia 00d0.ff78.4301)
```

 S2
   
 ```S2#sh inter f0/1
 FastEthernet0/1 is up, line protocol is up (connected)
  Hardware is Lance, address is 0001.63d1.1501 (bia 0001.63d1.1501)

  ```
  
  Назовите адреса оборудования во второй строке выходных данных команды (или зашитый адрес -bia)
  
 - MAC-адрес коммутатора S1 Fast Ethernet 0/1: 00d0.ff78.4301
 - MAC-адрес коммутатора S2 Fast Ethernet 0/1: 0001.63d1.1501

##### Шаг 2. Просмотрите таблицу MAC-адресов коммутатора.

a. Подключитесь к коммутатору S2 через консоль и войдите в привилегированный режим EXEC

b. В привелегированном режиме EXEC введите команду *show mac address-table* и нажмите клавишу ввода

Таблица MAC-адресов **до** тестирования сетевой связи с помощью эхо-запросов

```User Access Verification

Password: 

S2>en
Password: 
S2#sh mac address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    000c.8502.3ed2    DYNAMIC     Fa0/1
   1    0060.4743.aa40    DYNAMIC     Fa0/18
   1    00d0.ff78.4301    DYNAMIC     Fa0/1
   
```   

 - В таблице MAC-адресов указаны MAC-адреса, полученные динамически.
 - В таблице указаны MAC-адреса PC-A и PC-B
 - Если ранее MAC-адреса не были записаны, то по табличным данным можно ориентироваться на порты, то есть в какие интерфейсы устройства подключены
 - Такое решение подходит только для самой простой сети, где через один интерфейс не будет подключено несколько устройств
 

##### Шаг 3.Очистите таблицу MAC-адресов коммутатора S2 и снова отобразите таблицу MAC-адресов

a. В привилегированном режиме EXEC введите команду *clear mac address-table dynamic* и нажмите клавишу Enter

```
S2#clear mac-address-table dynamic 
S2#sh mac address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

 ```

b. Снова быстро введите команду *show mac address-table*

```S2#sh mac address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    00d0.ff78.4301    DYNAMIC     Fa0/1
```

После повторного введения той же команды появился только собственный MAC-адрес устройства.



##### Шаг 4.  С компьютера PC-B отправьте эхо-запросы устройствам сети и просмотрите таблицу MAC-адресов коммутатора.

a. На компьютере PC-B введите команду *arp -a*

```C:\>  arp -a
No ARP Entries Found
```
После первичного введения команды *arp -a* не было получено ни одной пары IP- и -MAC-адресов устройств через протокол ARP

b. Из командной строки PC-B отправьте эхо-запросы на все устройства сети

```
C:\>ping 192.168.1.12

Pinging 192.168.1.12 with 32 bytes of data:

Request timed out.
Reply from 192.168.1.12: bytes=32 time<1ms TTL=255
Reply from 192.168.1.12: bytes=32 time<1ms TTL=255
Reply from 192.168.1.12: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.12:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

C:\>ping 192.168.1.11

Pinging 192.168.1.11 with 32 bytes of data:

Request timed out.
Reply from 192.168.1.11: bytes=32 time=1ms TTL=255
Reply from 192.168.1.11: bytes=32 time<1ms TTL=255
Reply from 192.168.1.11: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.1.11:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

C:\>ping 192.168.1.1

Pinging 192.168.1.1 with 32 bytes of data:

Reply from 192.168.1.1: bytes=32 time<1ms TTL=128
Reply from 192.168.1.1: bytes=32 time<1ms TTL=128
Reply from 192.168.1.1: bytes=32 time=1ms TTL=128
Reply from 192.168.1.1: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.1.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

``` 
 
Ответ получен от всех устройств


c. Подключившись через консоль к коммутатору S2, введите команду *show mac address-table*

```S2#sh mac address-table 
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----

   1    000c.8502.3ed2    DYNAMIC     Fa0/1
   1    0060.4743.aa40    DYNAMIC     Fa0/18
   1    00d0.ff78.4301    DYNAMIC     Fa0/1
   1    00e0.a341.84ae    DYNAMIC     Fa0/1
```
В таблицу добавлен MAC-адрес интерфейса коммутатора S2

``` C:\>arp -a
  Internet Address      Physical Address      Type
  192.168.1.1           000c.8502.3ed2        dynamic
  192.168.1.11          00e0.a341.84ae        dynamic
  192.168.1.12          0004.9ac8.6b6b        dynamic
 ```
  После отправки эхо-запросов на устройства сети, в ARP-кэше компьютера PC-B появились пары IP-и MAC-адресов устройств сети, кроме самого компьютера PC-B


#### Вопрос для повторения

В сетях Ethernet данные передаются на устройства  по соответствующим MAC-адресам. Для этого коммутаторы и компьютеры динамически создают ARP-кэш и таблицы MAC-адресов.  Если компьютеров в сети немного, эта процедура выглядит достаточно простой. Какие сложности могут возникнуть с больших сетях?

- таблица MAC-адресов кэшируется и обновляется каждые 2-10 мин. В больших сетях, запросы, которые будут направлять устройства хостам, будут влиять на производительность сети, то есть перегружать ее работу не всегда необходимыми запросами.
- Запрос отправляется в сеть, если в таблице устройства нет необходимого адреса и кэш обновляется
