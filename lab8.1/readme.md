## Лабораторная работа - Реализация DHCPv4

#### Топология

![](http://dl3.joxi.net/drive/2021/12/12/0050/1314/3282210/10/0ad11c77aa.jpg)

#### 1. Таблица адресации


| Устройство | Интерфейс| IP-адрес | Маска подсети | Шлюз по умолчанию |
 | ------------- |:------------------:| -----:|-----:|-----:|
 | R1 | G0/0/0 | 10.0.0.1 | 255.255.255.252 | |
 |  | G0/0/1 | -  | - | |
 |  | G0/0/1.100 | 192.168.1.1  | 255.255.255.192 | |
 |  | G0/0/1.200 | 192.168.1.65 | 255.255.255.224 | |
 |  | G0/0/1.1000 | -  | - | |
 | R2 | G0/0/0 | 10.0.0.2 | 255.255.255.252 | |
 |  | G0/0/1 | 192.168.1.97  | 255.255.255.240| |
 | S1 | VLAN 200 | -| - ||
 | S2 | VLAN 1| |  ||
 | PC-A | NIC | DHCP | DHCP |DHCP |
 | PC-B | NIC | DHCP | DHCP |DHCP |

#### 2. Таблица VLAN

| VLAN | Имя | Назначенный интерфейс |
| ------------- |:------------------:| -----:|
| 1 | Нет | S2: F0/18 |
| 100 | Клиенты | S1: F0/6 |
| 200 |Управление | S1: VLAN 200 |
| 999 |Parking_lot | S1: F0/1-4, F0/-24, G0/1-2 |
| 1000 |Собственная |  |


#### 3. Задачи 


Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Настройка и проверка двух серверов DHCPv4 на R1

Часть 3. Настройка и проверка DHCP-ретрансляции на R2


#### Инструкции

#### Часть 1. Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.

#### Шаг 1. Создание схемы адресации

Подсеть сети 192.168.1.0/24 в соответствии со следующими требованиями:

a. Одна подсеть «Подсеть A», поддерживающая 58 хостов (клиентская VLAN на R1).

Подсеть A

Запишите первый IP-адрес в таблице адресации для R1 G0/0/1.100 . Запишите второй IP-адрес в таблице адресов для S1 VLAN 100 и введите соответствующий шлюз по умолчанию.

b. Одна подсеть «Подсеть B», поддерживающая 28 хостов (управляющая VLAN на R1).

Подсеть B:

Запишите первый IP-адрес в таблице адресации для R1 G0/0/1.200. Запишите второй IP-адрес в таблице адресов для S1 VLAN 200 и введите соответствующий шлюз по умолчанию.

c. Одна подсеть «Подсеть C», поддерживающая 12 узлов (клиентская сеть на R2).

Подсеть C:

Запишите первый IP-адрес в таблице адресации для R2 G0/0/1.

a-c Выполнено


#### Шаг 2. Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели.

#### Шаг 3. Произведите базовую настройку маршрутизаторов.

a. Назначьте маршрутизатору имя устройства.

Откройте окно конфигурации

b. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c. Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d. Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e. Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f. Зашифруйте открытые пароли.

g. Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h. Сохраните текущую конфигурацию в файл загрузочной конфигурации.

i. Установите часы на маршрутизаторе на сегодняшнее время и дату.

Примечание. Вопросительный знак (?) позволяет открыть справку с правильной последовательностью параметров, необходимых для выполнения этой команды.

a-i 

```
Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hos
Router(config)#hostname R2
R2(config)#no ip domain-name
R2(config)#ena
R2(config)#enable secret class
R2(config)#line con 0	
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#line vty 0 4
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#service password-encryption 
R2(config)#banner motd *Unauthorized access is strictly prohibited*
R2(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R2(config)#exit
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#clock set 15:39:00 dec 11 2021
R2#exit

```

#### Шаг 4. Настройка маршрутизации между сетями VLAN на маршрутизаторе R1

a. Активируйте интерфейс G0/0/1 на маршрутизаторе.

b. Настройте подинтерфейсы для каждой VLAN в соответствии с требованиями таблицы IP-адресации. Все субинтерфейсы используют инкапсуляцию 802.1Q и назначаются первый полезный адрес из вычисленного пула IP-адресов. Убедитесь, что подинтерфейсу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.

c. Убедитесь, что вспомогательные интерфейсы работают.

a-b

```

R1(config)# int g0/0/1
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#int g0/0/1.100 
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.100, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.100, changed state to up

R1(config-subif)#description CLIENTS
R1(config-subif)#encapsulation dot1Q 100
R1(config-subif)#ip add 192.168.1.1 255.255.255.192
R1(config-subif)#no sh
R1(config-subif)#exit

R1(config)#int g0/0/1.200
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.200, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.200, changed state to up

R1(config-subif)# description MANAGEMENT
R1(config-subif)#enc
R1(config-subif)#encapsulation do
R1(config-subif)#encapsulation dot1Q 200
R1(config-subif)#ip add 192.168.1.65 255.255.255.224
R1(config-subif)#no sh

R1(config)#int g0/0/1.1000
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.1000, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.1000, changed state to up

R1(config-subif)#desc
R1(config-subif)#description NATIVE
R1(config-subif)#encapsulation dot1Q 1000 native
R1(config-subif)#no sh

```

c.

![](http://joxi.ru/V2VJNgot8gWWe2.jpg)



#### Шаг 5. Настройте G0/1 на R2, затем G0/0/0 и статическую маршрутизацию для обоих маршрутизаторов


a. Настройте G0/0/1 на R2 с первым IP-адресом подсети C, рассчитанным ранее.

```
R2(config)#int g0/0/1
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R2(config-if)#ip add 192.168.1.97 255.255.255.240
R2(config-if)#

```

b. Настройте интерфейс G0/0/0 для каждого маршрутизатора на основе приведенной выше таблицы IP-адресации.

c. Настройте маршрут по умолчанию на каждом маршрутизаторе, указываемом на IP-адрес G0/0/0 на другом маршрутизаторе.

```

R1(config)#int  g0/0/0
R1(config-if)#no sh 
R1(config-if)#ip ad 10.0.0.1 255.255.255.252
R1(config-if)#exit
R1(config) ip route 0.0.0.0 0.0.0.0 10.0.0.2 

```

```

R2(config)#int  g0/0/0
R2(config-if)#no sh 
R2(config-if)#ip ad 10.0.0.2 255.255.255.252
R2(config-if)#exit
R2(config) ip route 0.0.0.0 0.0.0.0 10.0.0.1 

```


d. Убедитесь, что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1.

![](http://joxi.ru/1A503akTzqyvw2.jpg)

```


```

* Маршрутизатор R2 настроен аналогичным образом.

#### Шаг 6. Настройте базовые параметры каждого коммутатора.

a. Присвойте коммутатору имя устройства.

Откройте окно конфигурации

b. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.
c. Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d. Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e. Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f. Зашифруйте открытые пароли.

g. Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h. Сохраните текущую конфигурацию в файл загрузочной конфигурации.

i. Установите часы на маршрутизаторе на сегодняшнее время и дату.

Примечание. Вопросительный знак (?) позволяет открыть справку с правильной последовательностью параметров, необходимых для выполнения этой команды.

j. Скопируйте текущую конфигурацию в файл загрузочной конфигурации.


```
Switch>
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hos
Switch(config)#hostname S1
S1(config)#no ip domain-name
S1(config)#enable sec class
S1(config)#line con 0
S1(config-line)#password cisco
S1(config-line)login
S1(config-line)#logg synchronous 
S1(config-line)#exec-timeout 0 0
S1(config-line)#exit
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#service password-encryption 
S1(config)#banner motd *Unauthorized access is strictly prohibited*
S1(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
S1(config)#ntp ser
S1(config)#ntp server ?
  Hostname or A.B.C.D  IP address of peer


S1(config)#ntp server 10.0.0.1

```

* Коммутатор S2  настроен аналогичным образом

####  Шаг 7. Создайте сети VLAN на коммутаторе S1.

Примечание. S2 настроен только с базовыми настройками.


d. Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты.


a.Создайте необходимые VLAN на коммутаторе 1 и присвойте им имена из приведенной выше таблицы.

b. Настройте и активируйте интерфейс управления на S1 (VLAN 200), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того установите шлюз по умолчанию на S1.

a-b

```

S1(config)#vlan 100
S1(config-vlan)#name CLIENTS
S1(config-vlan)#exit
S1(config)#vlan 200
S1(config-vlan)#name MANAGEMENT
S1(config-vlan)#exit
S1(config)#vlan 999
S1(config-vlan)#name Parking_lot
S1(config-vlan)#exit
S1(config)#vlan 1000
S1(config-vlan)#name NATIVE
S1(config-vlan)#exit
S1(config)int vlan 200
S1(config-if)ip address 192.168.1.66 255.255.255.224
S1(config-if)exit
S1(config)ip default-gateway 192.168.1.65

```
c. Настройте и активируйте интерфейс управления на S2 (VLAN 1), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того, установите шлюз по умолчанию на S2

```

S2(config)#interface vlan 1
S2(config-if)#ip address 192.168.1.98 255.255.255.240
S2(config-if)#exit
S2(config)#ip default-gateway 192.168.1.97

```

d. Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты.

```

S1(config)#int ra f0/1-4, f0/7-24, g0/1-2
S1(config-if-range)#sh
.
.
S1(config-if-range)#sw mo ac
S1(config-if-range)#sw ac vlan 999

```

```

S2(config)#int ra f0/1-4, f0/6-17, f0/19-24, g0/1-2
S2(config-if-range)#sh

```

#### Шаг 8. Назначьте сети VLAN соответствующим интерфейсам коммутатора.

a. Назначьте используемые порты соответствующей VLAN (указанной в таблице VLAN выше) и настройте их для режима статического доступа.

```

S1(config)#int f0/6
S1(config-if)#sw mo ac
S1(config-if)#sw ac vlan 100

```

```

S2(config)#int f0/18
S2(config-if)#sw mo ac
S2(config-if)#sw ac vlan 1

```

b. Убедитесь, что VLAN назначены на правильные интерфейсы.

![](http://joxi.ru/L21qKdOUz7yv42.jpg)

![](http://joxi.ru/12MnONoHwVbGqA.jpg)


Вопрос: Почему интерфейс F0/5 указан в VLAN 1? 

Коммутатор S2 имеет только базовые настройки, поэтому интерфейс F0/5 находится во VLAN 1, то есть NATIVE


#### Шаг 9. Вручную настройте интерфейс S1 F0/5 в качестве транка 802.1Q.

a. Измените режим порта коммутатора, чтобы принудительно создать магистральный канал.

b. В рамках конфигурации транка установите для native VLAN значение 1000.

c. В качестве другой части конфигурации магистрали укажите, что VLAN 100, 200 и 1000 могут проходить по транку.

d. Сохраните текущую конфигурацию в файл загрузочной конфигурации.

e. Проверьте состояние транка.

Вопрос: Какой IP-адрес был бы у ПК, если бы он был подключен к сети с помощью DHCP

192.168.1.2


```

S1(config)#int f0/5
S1(config-if) sw mo tr
S1(config-if)#sw tru all
S1(config-if)#sw tru allowed add vlan 100, 200, 1000
S1(config-if)#sw trunk native vlan 1000
S1(config-if)#end

```

![](http://joxi.ru/ZrJRa0oCbV0Var.jpg)

### Часть 2. Настройка и проверка двух серверов DHCPv4 на R1

В части 2 необходимо настроить и проверить сервер DHCPv4 на R1. Сервер DHCPv4 будет обслуживать две подсети, подсеть A и подсеть C.

#### Шаг 1. Настройте R1 с пулами DHCPv4 для двух поддерживаемых подсетей. Ниже приведен только пул DHCP для подсети A

a. Исключите первые пять используемых адресов из каждого пула адресов.

Откройте окно конфигурации

b. Создайте пул DHCP (используйте уникальное имя для каждого пула).

c. Укажите сеть, поддерживающую этот DHCP-сервер.

d. В качестве имени домена укажите CCNA-lab.com.

e. Настройте соответствующий шлюз по умолчанию для каждого пула DHCP.

f. Настройте время аренды на 2 дня 12 часов и 30 минут.

g. Затем настройте второй пул DHCPv4, используя имя пула R2_Client_LAN и вычислите сеть, маршрутизатор по умолчанию, и используйте то же имя домена и время аренды, что и предыдущий пул DHCP.

```

Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.5
R1(config)#ip dhcp excluded-address 192.168.1.65 192.168.1.69
R1(config)#ip dhcp excluded-address 192.168.1.97 192.168.1.101
R1(config)#ip dhcp pool SUBNET-A
R1(dhcp-config)#network 192.168.1.0 255.255.255.192
R1(dhcp-config)#default-router 192.168.1.1
R1(dhcp-config)#lease 2 12 30

R1(config)#ip dhcp pool SUBNET-B
R1(dhcp-config)#network 192.168.1.64 255.255.255.224
R1(dhcp-config)#default-router 192.168.1.65
R1(dhcp-config)#domain-name CCNA-lab.com
R1(dhcp-config)#domain-name CCNA-lab.com
R1(dhcp-config)#lease 2 12 30

R1(config)#ip dh
R1(config)#ip dhcp pool R2_Client_LAN
R1(dhcp-config)#default-router 192.168.1.97
R1(dhcp-config)#network 192.168.1.96 255.255.255.240
R1(dhcp-config)#domain-name CCNA-lab.com
R1(dhcp-config)#lease 2 12 30

```

![](http://joxi.ru/DrlD893UGWELlr.jpg)




![]()
![]()

