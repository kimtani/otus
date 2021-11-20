
### Лабораторная работа - Настройка протоколов CDP, LLDP и NTP

#### Топология

![](http://joxi.ru/VrwDB9JUj6nx1m.jpg)

| Устройство | Интерфейс | IP- адрес | Маска подсети | Шлюз по умолчанию |
 | ------------- |:------------------:| -----:|-----:|-----:|
 | R1 | Loopback 1 | 172.16.1.1| 255.255.255.0 | |
 |  |G0/0/1| 10.22.0.1|255.255.255.0||
 |S1 |SVI VLAN 1| 10.22.0.2|255.255.255.0|10.22.0.1|
 | S |SVI VLAN 1|10.22.0.3 |255.255.255.0|10.22.0.1|


#### Задачи

#### Часть 1. Создание сети и настройка основных параметров устройства

#### Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP

#### Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP

#### Часть 4. Настройка и проверка NTP

---------------------------------

#### Часть 1. Создание сети и настройка основных параметров устройства
 
Шаг 1. Создайте сеть согласно топологии.

Шаг 2. Настройте базовые параметры для маршрутизатора.

a.	Назначьте маршрутизатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Настройка интерфейсов, перечисленных в таблице выше

i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.


```
Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#no ip domain-lookup 
R1(config)#ena
R1(config)#enable secret class
R1(config)#line con 0
R1(config-line)#logg synchronous 
R1(config-line)#exec
R1(config-line)#exec-timeout 0 0
R1(config-line)#password cisco
R1(config-line)#exit
R1(config)#service password-encryption 
R1(config)#banner motd *Authorized users only!  Violaters will be shot on sight!*
R1(config)#int loopback 1

R1(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R1(config-if)#ip add 172.16.1.1 255.255.255.0
R1(config-if)#exit
R1(config)#int g0/0/1
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#ip add 10.22.0.1 255.255.255.0
R1(config-if)#exit
R1(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R1(config)#

```

#### Шаг 3. Настройте базовые параметры каждого коммутатора.

a.	Присвойте коммутатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер, который предупреждает всех, кто обращается к устройству, видит баннерное сообщение «Только авторизованные пользователи!».  

h.	Отключите неиспользуемые интерфейсы

i.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.



```
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#no ip domain-lookup 
Switch(config)#hostname S1
S1(config)#ena secret class
S1(config)#line con 0
S1(config-line)#pass cisco
S1(config-line)#logg syn
S1(config-line)#exec-ti 0 0
S1(config-line)#exit
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#ser password-encryption 
S1(config)#banner motd *Authorized users only!  Violaters will be shot on sight*
S1(config)#int  ra f0/2-4
S1(config-if-range)#sh

%LINK-5-CHANGED: Interface FastEthernet0/3, changed state to administratively down

%LINK-5-CHANGED: Interface FastEthernet0/4, changed state to administratively down

S1(config-if)#exit
S1(config)#int ra f0/6-24
S1(config-if-range)#sh


S1(config-if)#exit

S1(config)#int ra g0/1-2
S1(config-if-range)#no sh
S1(config-if-range)#sh

%LINK-5-CHANGED: Interface GigabitEthernet0/1, changed state to administratively down

%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down

```

#### Часть 2. Обнаружение сетевых ресурсов с помощью протокола CDP



a.	На R1 используйте соответствующую команду show cdp, чтобы определить, сколько интерфейсов включено CDP, сколько из них включено и сколько отключено.

```
 R1#sh cdp ne
Capability Codes: R - Router, T - Trans Bridge, B - Source Route Bridge
                  S - Switch, H - Host, I - IGMP, r - Repeater, P - Phone
Device ID    Local Intrfce   Holdtme    Capability   Platform    Port ID
S1           Gig 0/0/1        143            S       2960        Fas 0/5

```
 
Сколько интерфейсов участвует в объявлениях CDP? Какие из них активны?

 - Один интерфейс участвует в объявлениях CDP. Один активный.


b.	На R1 используйте соответствующую команду *show cdp*, чтобы определить версию IOS, используемую на S1.

```
R1#sh cdp nei detail

``` 

Какая версия IOS  используется на S1

![](http://joxi.ru/a2XgqGoUl6qpdm.jpg)

c.	На S1 используйте соответствующую команду show cdp, чтобы определить, сколько пакетов CDP было выданных.

![](http://joxi.ru/L21qKdOUzK79q2.jpg)

 
d.	Настройте SVI для VLAN 1 на S1 и S2, используя IP-адреса, указанные в таблице адресации выше. Настройте шлюз по умолчанию для каждого коммутатора на основе таблицы адресов.
 
e.	На R1 выполните команду show cdp entry S1 . 

![](http://joxi.ru/D2Pkx0oCBLDVBm.jpg)

Какие дополнительные сведения доступны теперь?

Введите ваш ответ здесь.
 
  



f.	Отключить CDP глобально на всех устройствах. 

Команда *no cdp run* для всех устройств

``` 
R1(config)#no cdp run

``` 
 
#### Часть 3. Обнаружение сетевых ресурсов с помощью протокола LLDP


a.	Введите соответствующую команду lldp, чтобы включить LLDP на всех устройствах в топологии.

``` 
S1(config)# lldp run

``` 

b.	На S1 выполните соответствующую команду lldp, чтобы предоставить подробную информацию о S2. 

```
S1# show lldp entry S2

Capability codes:
    (R) Router, (B) Bridge, (T) Telephone, (C) DOCSIS Cable Device
    (W) WLAN Access Point, (P) Repeater, (S) Station, (O) Other
------------------------------------------------
Local Intf: Fa0/3
Chassis id: 1c1d.8686.1d80
Port id: Fa0/3
Port Description: FastEthernet0/3
System Name: S2
System Description: 
Cisco IOS Software, C2960 Software (C2960-LANBASEK9-M), Version 15.2(2)E9, RELEASE SOFTWARE (fc4)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2018 by Cisco Systems, Inc.
Compiled Sat 08-Sep-18 16:34 by prod_rel_team
Time remaining: 113 seconds
System Capabilities: B
Enabled Capabilities: B
Management Addresses:
    IP: 10.22.0.3
Auto Negotiation - supported, enabled
Physical media capabilities:
    100base-TX(FD)
    100base-TX(HD)
    10base-T(FD)
    10base-T(HD)
Media Attachment Unit type: 16
Vlan ID: 1
Total entries displayed: 1

```


Что такое chassis ID  для коммутатора S2?

Информация, полученная из кадра коммутатора S1.
 

c.	Соединитесь через консоль на всех устройствах и используйте команды LLDP, необходимые для отображения топологии физической сети только из выходных данных команды show.
 

![](http://joxi.ru/5mdVO36iaGRNDA.jpg)

![](http://joxi.ru/p27jn3yUnwlQZm.jpg)

![](http://joxi.ru/GrqDb9kURZe18A.jpg)



#### Часть 4. Настройка NTP

#### Шаг 1. Выведите на экран текущее время.


```
R1#show clock
*9:18:10.489 UTC Mon Mar 1 1993

```

	
			
#### Шаг 2. Установите время.

С помощью команды *clock set* установите время на маршрутизаторе R1. Введенное время должно быть в формате UTC. 

```
R1#clock set 23:35:00 20 nov 2021

```
 
#### Шаг 3. Настройте главный сервер NTP.

Настройте R1 в качестве хозяина NTP с уровнем слоя 4.

```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#clock timezone MSK 3
R1(config)#ntp master 4

```
 
#### Шаг 4. Настройте клиент NTP.

a.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время. Запишите текущее время,  в следующей таблице.


```
S1#sh clock de
*10:58:27.426 UTC Tue Mar 2 1993
Time source is hardware calendar

```


```
S2#sh clock de
*10:58:37.537 UTC Tue Mar 2 1993
Time source is hardware calendar

```
		
b.	Настройте S1 и S2 в качестве клиентов NTP. Используйте соответствующие команды NTP для получения времени от интерфейса G0/0/1 R1, а также для периодического обновления календаря или аппаратных часов коммутатора.

Настройка NTP для S1, аналогично для S2

```
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#clock timezone MSK 3
S1(config)#ntp server 10.22.0.1
S1(config)#ntp update-calendar
S1(config)#exit

```

#### Шаг 5. Проверьте настройку NTP.

a.	Используйте соответствующую команду show , чтобы убедиться, что S1 и S2 синхронизированы с R1.
Примечание. Синхронизация метки времени на маршрутизаторе R2 с меткой времени на маршрутизаторе R1 может занять несколько минут.

Для S1

![](http://joxi.ru/Vm6EOR1TRMOkKr.jpg)	


	
Для S2

![](http://joxi.ru/823MaKXCaRoOqr.jpg)
	
	

b.	Выполните соответствующую команду на S1 и S2, чтобы просмотреть настроенное время и сравнить ранее записанное время.



![](http://joxi.ru/eAOxXNoi6B0bPr.jpg)	

![](http://joxi.ru/zANXnjoT8MQg7m.jpg)		
	
#### Вопрос для повторения

Для каких интерфейсов в пределах сети не следует использовать протоколы обнаружения сетевых ресурсов? Поясните ответ.

 - Для неиспользуемых интерфейсов. Такие интерфейсы рекомендовано отключить, чтобы защититься от несанкционированного подключения, и избежать утечки данных сетевых ресурсов.

