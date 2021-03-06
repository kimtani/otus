## Настройка и проверка расширенных списков контроля доступа.

### Лабораторная работа выполнена для курса [**Network Engineer. OTUS**](https://otus.ru/lessons/network-engineer-specialization/#rec313881739)


#### Топология

![](http://joxi.ru/ZrJRa0oCb4Jear.jpg)

#### Таблица адресации


 | Устройство | Интерфейс | IP- адрес |Маска подсети | Шлюз по умолчанию | 
 | ------------- |:------------------:| -----:|-----:|-----:| 
 | R1 | G0/0/1 |  | | | 
 |  |G0/0/1.20|10.20.0.1| 255.255.255.0| | 
 |  |G0/0/1.30|10.30.0.1 | 255.255.255.0| | 
 |  |G0/0/1.40|10.40.0.1|255.255.255.0| | 
 |  |G0/0/1.1000||| |
 |  |Loopback 1 |172.16.1.1|255.255.255.0| |
 | R2 | G0/0/1 |10.20.0.4 |255.255.255.0 | | 
 | S1 |VLAN 20 | 10.20.0.2|255.255.255.0|10.20.0.1 | 
 | S2 |VLAN 20| 10.20.0.3|255.255.255.0| 10.20.0.1| 
 | PC-A | NIC | 10.30.0.10| 255.255.255.0| 10.30.0.1| 
 | PC-B | NIC | 10.40.0.10|255.255.255.0 | 10.40.0.1| 

#### Таблица VLAN

| VLAN | Имя  | Назначенный интерфейс |
 | ------------- |:------------------:| -----:|
 | 20 | Management| S2: F0/5|
 | 30 | Operations | S1: F0/6 |
 | 40 | Sales| S2: F0/18 |
 | 999 |Parking_Lot| S1:F0/2-4,F0/7-24, G0/0/1-2|
  |||S2: F0/2-14,F0/6-17,F0/19-24, G0/0/1-2|
 | 1000 |Собственная| - |






#### Задачи

####  Часть 1. Создание сети и настройка основных параметров устройства

####  Часть 2. Настройка и проверка списков расширенного контроля доступа

------------------------------------------

####  Инструкции

#### Часть 1. Создание сети и настройка основных параметров устройства

#### Шаг 1. Создайте сеть согласно топологии.

#### Шаг 2. Произведите базовую настройку маршрутизаторов.

a.	Назначьте маршрутизатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

#### Шаг 3. Настройте базовые параметры каждого коммутатора.

a.	Присвойте коммутатору имя устройства.

b.	Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c.	Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d.	Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e.	Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f.	Зашифруйте открытые пароли.

g.	Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.


#### Часть 2. Настройка сетей VLAN на коммутаторах.

#### Шаг 1. Создайте сети VLAN на коммутаторах.

a.	Создайте необходимые VLAN и назовите их на каждом коммутаторе из приведенной выше таблицы.

b.	Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе в таблице адресации. 

c.	Назначьте все неиспользуемые порты коммутатора VLAN Parking Lot, настройте их для статического режима доступа и административно деактивируйте их.


#### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора.

a.	Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.

b.	Выполните команду show vlan brief, чтобы убедиться, что сети VLAN назначены правильным интерфейсам.

S1 настроен; Интерфейсы Vlan ParkingLot деактивированы командой *shutdown*

![](http://joxi.ru/bmoeq9XF7ZoPpA.jpg)

S2 настроен; Интерфейсы Vlan ParkingLot деактивированы командой *shutdown*

![](http://joxi.ru/823MaKXCavKQNr.jpg)



#### Часть 3. ·Настройте транки (магистральные каналы).

#### Шаг 1. Вручную настройте магистральный интерфейс F0/1.

a.	Измените режим порта коммутатора на интерфейсе F0/1, чтобы принудительно создать магистральную связь. Не забудьте сделать это на обоих коммутаторах.

b.	В рамках конфигурации транка установите для native vlan значение 1000 на обоих коммутаторах. При настройке двух интерфейсов для разных собственных VLAN сообщения об ошибках могут отображаться временно.

c.	В качестве другой части конфигурации транка укажите, что VLAN 10, 20, 30 и 1000 разрешены в транке.

d.	Выполните команду show interfaces trunk для проверки портов магистрали, собственной VLAN и разрешенных VLAN через магистраль.

![](http://joxi.ru/krDW60oigaLRZA.jpg)

![](http://joxi.ru/Vm6EOR1TRo0Ojr.jpg)

#### Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1.

a.	Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.

b.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.

c.	Используйте команду show interfaces trunk для проверки настроек транка.


#### Часть 4. Настройте маршрутизацию.

#### Шаг 1. Настройка маршрутизации между сетями VLAN на R1.

a.	Активируйте интерфейс G0/0/1 на маршрутизаторе.

b.	Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфейс для собственной VLAN не имеет назначенного IP-адреса. Включите описание для каждого подинтерфейса.

c.	Настройте интерфейс Loopback 1 на R1 с адресацией из приведенной выше таблицы.

d.	С помощью команды show ip interface brief проверьте конфигурацию подынтерфейса.

```
R1(config)#int g0/0/1.20
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.20, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.20, changed state to up

R1(config-subif)#enca dot1q 20
R1(config-subif)#ip ad 10.20.0.1 255.255.255.0
R1(config-subif)#no sh
R1(config-subif)#exit
R1(config)#int g0/0/1.30
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.30, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.30, changed state to up

R1(config-subif)#enc dot1q 30
R1(config-subif)#ip add 10.30.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.40 
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.40, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.40, changed state to up

R1(config-subif)#enc dot1Q 40
R1(config-subif)#ip add 10.40.0.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.1000
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.1000, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.1000, changed state to up

R1(config-subif)#exit
R1(config)#int loopback 1

R1(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

	
R1(config-if)#ip ad 172.16.1.1 255.255.255.0

```
```
R1(config)#int g0/0/1.20
R1(config-subif)#description to VLAN 20
R1(config-subif)#int g0/0/1.30
R1(config-subif)#description to VLAN 30
R1(config-subif)#int g0/0/1.40
R1(config-subif)#description to VLAN 40
R1(config-subif)#int g0/0/1.1000
R1(config-subif)#description to VLAN 1000

R1(config)#int lo 1
R1(config-if)#ip add 172.16.1.1 255.255.255.0


```

![](http://joxi.ru/v29zxn8HR6KXwA.jpg)






#### Шаг 2. Настройка интерфейса R2 g0/0/1 с использованием адреса из таблицы и маршрута по умолчанию
с адресом следующего перехода 10.20.0.1

```
R2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R2(config)#int g0/0/1
R2(config-if)#ip add 10.20.0.4 255.255.255.0
R2(config-if)#ip route 0.0.0.0 0.0.0.0 10.20.0.1
R2(config)#exit
R2#
%SYS-5-CONFIG_I: Configured from console by console

```

### Часть 5. Настройте удаленный доступ

#### Шаг 1. Настройте все сетевые устройства для базовой поддержки SSH.

a.	Создайте локального пользователя с именем пользователя SSHadmin и зашифрованным паролем $cisco123!

b.	Используйте ccna-lab.com в качестве доменного имени.

c.	Генерируйте криптоключи с помощью 1024 битного модуля.

d.	Настройте первые пять линий VTY на каждом устройстве,

чтобы поддерживать только SSH-соединения и с локальной аутентификацией.


```
R1(config)#ip ssh ver 2
Please create RSA keys (of at least 768 bits size) to enable SSH v2.
R1(config)#username admin password $cisco123!
R1(config)#ip domain-name ccna-lab.com
R1(config)#cry
R1(config)#crypto key gener rsa
The name for the keys will be: R1.ccna-lab.com
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

R1(config)#line vty 0 4
* Mar 1 0:1:27.872: %SSH-5-ENABLED: SSH 2 has been enabled
R1(config-line)#password $cisco123!
R1(config-line)#login
R1(config-line)#tra
R1(config-line)#transport input ssh
R1(config-line)#

```



#### Шаг 2. Включите защищенные веб-службы с проверкой подлинности на R1.

a.	Включите сервер HTTPS на R1.

R1(config)# ip http secure-server 

- выполнено

b.	Настройте R1 для проверки подлинности пользователей, пытающихся подключиться к веб-серверу.

R1(config)# ip http authentication local

- выполнено


![](http://joxi.ru/RmzD0VMUj9BljA.jpg)




#### Часть 6. Проверка подключения

#### Шаг 1. Настройте узлы ПК.

- выполнено

#### Шаг 2. Выполните следующие тесты. Эхозапрос должен пройти успешно.



|От	|Протокол|	Назначение|
| ------------- |:------------------:| -----:|
|PC-A	|Ping	|10.40.0.10|
|PC-A|	Ping|	10.20.0.1|
|PC-B	|Ping	|10.30.0.10|
|PC-B|	Ping|	10.20.0.1|
|PC-B	|Ping	|172.16.1.1|
|PC-B|	HTTPS|	10.20.0.1|
|PC-B|	HTTPS|	172.16.1.1|
|PC-B	|SSH|	10.20.0.1|
|PC-B|	SSH	|172.16.1.1|

![](http://joxi.ru/Q2K3RBohyVKJk2.jpg)



![](http://joxi.ru/nAyD0MgUaxJea2.jpg)
![](http://joxi.ru/MAjDy9aU16WkOm.jpg)
![](http://joxi.ru/v29zxn8HR08pJA.jpg)

![](http://joxi.ru/nAyD0MgUax7jx2.jpg)
![](http://joxi.ru/n2YMQVoC7L3ev2.jpg)



#### Часть 7. Настройка и проверка списков контроля доступа (ACL)

При проверке базового подключения компания требует реализации следующих политик безопасности:

 - Политика 1. Сеть Sales не может использовать SSH в сети Management (но в  другие сети SSH разрешен).

 - Политика 2. Сеть Sales не имеет доступа к IP-адресам в сети Management с помощью любого веб-протокола (HTTP/HTTPS). Сеть Sales также не имеет доступа к интерфейсам R1 с помощью любого веб-протокола. Разрешён весь другой веб-трафик (обратите внимание — Сеть Sales  может получить доступ к интерфейсу Loopback 1 на R1).
 
 - Политика 3. Сеть Sales не может отправлять эхо-запросы ICMP в сети Operations или Management. Разрешены эхо-запросы ICMP к другим адресатам. 
 
  - Политика 4: Cеть Operations не может отправлять ICMP эхо-запросы в сеть Sales. Разрешены эхо-запросы ICMP к другим адресатам.
 

#### Шаг 1. Проанализируйте требования к сети и политике безопасности для планирования реализации ACL.

- Для сети SALES запретить SSH в сеть MANAGEMENT
- Для сети SALES разрешить SSH  в другие сети
- Для сети SALES запретить HTTP/HTTPS в сеть MANAGEMENT
- Для сети SALES запретить доступ к портам R1 по протоколам HTTP/HTTPS
- Для сети SALES разрешить доступ к Lo1 на R1
- Для сети SALES разрешен трафик по протоколам HTTP/HTTPS
- Для сети SALES запретить ICMP в сеть OPERATIONS
- - Для сети SALES запретить ICMP в сеть MANAGEMENT
- Для сети SALES разрешен трафик ICMP в другие сети
- Для сети OPERATIONS запретить ICMP в SALES
- Для сети OPERATIONS разрешено ICMP в другие сети
 
#### Шаг 2. Разработка и применение расширенных списков доступа, которые будут соответствовать требованиям политики безопасности.

 - for SALES

```

ip access-list extended SALES_FILTER
deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 22
permit tcp 10.40.0.0  0.0.0.255 any eq 22
deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq www
deny tcp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 eq 443
deny tcp 10.40.0.0 0.0.0.255 host 10.20.0.1 eq www
deny tcp 10.40.0.0 0.0.0.255 host 10.20.0.1 eq 443
deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq www
deny tcp 10.40.0.0 0.0.0.255 host 10.30.0.1 eq 443
deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq www
deny tcp 10.40.0.0 0.0.0.255 host 10.40.0.1 eq 443
permit ip 10.40.0.0 0.0.0.255 host 172.16.1.1 
permit tcp 10.40.0.0 0.0.0.255 any eq www
permit tcp 10.40.0.0 0.0.0.255 any eq 443
deny icmp 10.40.0.0 0.0.0.255 10.30.0.0 0.0.0.255 
deny icmp 10.40.0.0 0.0.0.255 10.20.0.0 0.0.0.255 
permit icmp 10.40.0.0 0.0.0.255 any

```

- for OPERATIONS

```

ip access-list extended OPERATION_FILTER
deny icmp 10.30.0.0 0.0.0.255 10.40.0.0 0.0.0.255
permit icmp 10.30.0.0 0.0.0.255 any
 
```
 
```

R1(config)#interface g0/0/1.40
R1(config-subif)#ip access-group SALES_FILTER in
R1(config)#interface g0/0/1.30
R1(config-subif)#ip access-group OPERATION_FILTER in

 ```

#### Шаг 3. Убедитесь, что политики безопасности применяются развернутыми списками доступа.

Выполните следующие тесты. Ожидаемые результаты показаны в таблице:

|От|	Протокол|	Назначение|	Результат|
| ------------- |:------------------:| -----:|-----:|
|PC-A	|Ping	|10.40.0.10|	Сбой|
|PC-A|	Ping	|10.20.0.1|	Успех|
|PC-B|	Ping|	10.30.0.10|	Сбой|
|PC-B|	Ping	|10.20.0.1|	Сбой|
|PC-B|Ping	|172.16.1.1|	Успех|
|PC-B	|HTTPS|	10.20.0.1|	Сбой|
|PC-B|	HTTPS|	172.16.1.1|	Успех|
|PC-B	|SSH|	10.20.0.4|	Сбой|
|PC-B	|SSH	|172.16.1.1|	Успех|

- Фактические результаты с ожидаемыми совпадают:

![](http://joxi.ru/1A503akTzBG312.jpg)
![](http://joxi.ru/DmB5Q0jUg9z7wr.jpg)
![](http://joxi.ru/Q2K3RBohynwKd2.jpg)
![](http://joxi.ru/brRy1pouL5YeqA.jpg)
![](http://joxi.ru/DrlD893UGY0aQr.jpg)
![](http://joxi.ru/BA0JLWltvGPLNm.jpg)
![](http://joxi.ru/Vm6EOR1TROjwar.jpg)



