### Конфигурация безопасности коммутатора

#### Топология

![](http://joxi.ru/82Q1kNluVXMwJ2.jpg)


#### Таблица адресации

| Устройство | Интерфейс/VLAN | IP-адрес | Маска подсети |
 | ------------- |:------------------:| -----:|-----:|
 | R1 | G0/0/1 | 192.168.10.1 | 255.255.255.0 | |
 |  |Loopback 0 | 10.10.1.1|255.255.255.0||
 | S1 | VLAN 10| 192.168.10.201| 255.255.255.0 |
 | S2 | VLAN 10| 192.168.10.202 | 255.255.255.0 |
 | PC-A | NIC | DHCP | 255.255.255.0 |
 | PC-B | NIC | DHCP | 255.255.255.0 |

#### Цели:

#### Часть 1. Настройка основного сетевого устройства

- Создайте сеть.

 - Настройте маршрутизатор R1.

 - Настройка и проверка основных параметров коммутатора
 
#### Часть 2. Настройка сетей VLAN

 - Сконфигруриуйте VLAN 10

 - Сконфигруриуйте SVI для VLAN 10

 - Настройте VLAN 333 с именем Native на S1 и S2.

 - Настройте VLAN 999 с именем ParkingLot на S1 и S2.

#### Часть 3: Настройки безопасности коммутатора.

 - Реализация магистральных соединений 802.1Q.

 - Настройка портов доступа

 - Безопасность неиспользуемых портов коммутатора

 - Документирование и реализация функций безопасности порта.

 - Реализовать безопасность DHCP snooping .

 - Реализация PortFast и BPDU Guard

 - Проверка сквозной связанности.

-----------------------------------------------

#### Инструкции:

#### Часть 1. Настройка основного сетевого устройства

#### Шаг 1. Создайте сеть.

a.Создайте сеть согласно топологии.

b. Инициализация устройств
Проверьте текущую конфигурацию на R1, используя следующую команду:
R1# show ip interface brief

c. Убедитесь, что IP-адресация и интерфейсы находятся в состоянии up / up (при необходимости
устраните неполадки).
Закройте окно настройки.

#### Шаг 2. Настройте маршрутизатор R1.

a.Загрузите следующий конфигурационный скрипт на R1.
Откройте окно конфигурации

```
enable
configure terminal
hostname R1
no ip domain lookup
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.10.201 192.168.10.202
!
ip dhcp pool Students
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
domain-name CCNA2.Lab-11.6.1
!
interface Loopback0
ip address 10.10.1.1 255.255.255.0
!
interface GigabitEthernet0/0/1
description Link to S1
ip dhcp relay information trusted
ip address 192.168.10.1 255.255.255.0
no shutdown
!
line con 0
logging synchronous
exec-timeout 0 0

```

b. Проверьте текущую конфигурацию на R1, используя следующую команду:

```
R1# show ip interface brief

```

c. Убедитесь, что IP-адресация и интерфейсы находятся в состоянии up / up (при необходимости
устраните неполадки).

a-c

![](http://joxi.ru/a2XgqGoUlPzQpm.jpg) 

#### Шаг 3. Настройка и проверка основных параметров коммутатора

a.Настройте имя хоста для коммутаторов S1 и S2.
Откройте окно конфигурации

b. Запретите нежелательный поиск в DNS.

c. Настройте описания интерфейса для портов, которые используются в S1 и S2.

d. Установите для шлюза по умолчанию для VLAN управления значение 192.168.10.1 на обоих
коммутаторах.

a-d


```
Switch>
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#line con 0
Switch(config-line)#logg syn
Switch(config-line)#exe
Switch(config-line)#exec-timeout 0 0
Switch(config-line)#exit
Switch(config)#hostname S1
S1(config)#no ip domain-name 
S1(config)#vlan 10
S1(config-vlan)#ip add 192.168.10.201

```
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#line con 0
Switch(config-line)#logg syn
Switch(config-line)#exec
Switch(config-line)#exec-timeout 0 0
Switch(config-line)#exit
Switch(config)#hos
Switch(config)#hostname S2
S2(config)#no ip domain-name
S2(config)#int vlan 10
S2(config-if)#no sh
S2(config-if)#ip add 192.168.10.202 255.255.255.0
S2(config-if)#ip de
S2(config-if)#ip default-ga
S2(config-if)#ip default-gate
S2(config-if)#ip default-gateway 192.168.10.1
S2(config)#int f0/18
S2(config-if)#des
S2(config-if)#description TO USER PC-B
S2(config-if)#exit
S2(config)#int f0/1
S2(config-if)#des
S2(config-if)#description TO S1
S2(config-if)#exit


#### Часть 2. Настройка сетей VLAN на коммутаторах.

#### Шаг 1. Сконфигруриуйте VLAN 10
Добавьте VLAN 10 на S1 и S2 и назовите VLAN - Management.

#### Шаг 2. Сконфигруриуйте SVI для VLAN 10
Настройте IP-адрес в соответствии с таблицей адресации для SVI для VLAN 10 на S1 и S2. Включите
интерфейсы SVI и предоставьте описание для интерфейса.

#### Шаг 3. Настройте VLAN 333 с именем Native на S1 и S2.

#### Шаг 4. Настройте VLAN 999 с именем ParkingLot на S1 и S2.

```
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#vlan 333
S1(config-vlan)#name NATIVE 
S1(config-vlan)#exit
S1(config)#vlan 999
S1(config-vlan)#name PARKINGLOT

```
 

#### Часть 3. Настройки безопасности коммутатора.

#### Шаг 1. Релизация магистральных соединений 802.1Q.

a. Настройте все магистральные порты Fa0/1 на обоих коммутаторах для использования VLAN 333 в
качестве native VLAN.

S1(config)#int f0/1
S1(config-if)#sw mo tr

S1(config-if)#
%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to down

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan10, changed state to up
S1(config-if)#sw tr native vlan 333
S1(config-if)#%SPANTREE-2-RECV_PVID_ERR: Received BPDU with inconsistent peer vlan id 1 on FastEthernet0/1 VLAN333.

%SPANTREE-2-BLOCK_PVID_LOCAL: Blocking FastEthernet0/1 on VLAN0333. Inconsistent local vlan.


S1(config-if)#sw tr allowed vlan 333
S1(config-if)#%SPANTREE-2-RECV_PVID_ERR: Received BPDU with inconsistent peer vlan id 1 on FastEthernet0/1 VLAN333.

%SPANTREE-2-BLOCK_PVID_LOCAL: Blocking FastEthernet0/1 on VLAN0333. Inconsistent local vlan.


S1(config-if)#%SPANTREE-2-UNBLOCK_CONSIST_PORT: Unblocking FastEthernet0/1 on VLAN0001. Port consistency restored.

%SPANTREE-2-UNBLOCK_CONSIST_PORT: Unblocking FastEthernet0/1 on VLAN0333. Port consistency restored.


S1(config-if)#sw tr all vlan 10
S1(config-if)#exit
S1(config)#do ping 192.168.10.202

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.10.202, timeout is 2 seconds:
..!!!
Success rate is 60 percent (3/5), round-trip min/avg/max = 0/0/0 ms

S1(config)#do ping 192.168.10.202

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.10.202, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/1/9 ms

S1(config)#

b. Убедитесь, что режим транкинга успешно настроен на всех коммутаторах.
S1# show interface trunk

c. Отключить согласование DTP F0/1 на S1 и S2.

d.Проверьте с помощью команды show interfaces.
S1# show interfaces f0/1 switchport | include Negotiation
Negotiation of Trunking: Off
S1# show interfaces f0/1 switchport | include Negotiation

#### Шаг 2. Настройка портов доступа

a. На S1 настройте F0/5 и F0/6 в качестве портов доступа и свяжите их с VLAN 10

b. На S2 настройте порт доступа Fa0/18 и свяжите его с VLAN 10

#### Шаг 3. Безопасность неиспользуемых портов коммутатора

a. На S1 и S2 переместите неиспользуемые порты из VLAN 1 в VLAN 999 и отключите
неиспользуемые порты.

b. Убедитесь, что неиспользуемые порты отключены и связаны с VLAN 999, введя команду show.
S1# show interfaces status

#### Шаг 4. Документирование и реализация функций безопасности порта.

Интерфейсы F0/6 на S1 и F0/18 на S2 настроены как порты доступа. На этом шаге вы также настроите
безопасность портов на этих двух портах доступа.

a.
На S1, введите команду show port-security interface f0/6 для отображения настроек по
умолчанию безопасности порта для интерфейса F0/6. Запишите свои ответы ниже.
Конфигурация безопасности порта по умолчанию
Функция
Настройка по умолчанию
Защита портов
Максимальное количество записей
MAC-адресов
Режим проверки на нарушение
безопасности
Aging Time
Aging Type
Secure Static Address Aging
Sticky MAC Address
b.
На S1 включите защиту порта на F0 / 6 со следующими настройками:
o
Максимальное количество записей MAC-адресов: 3
o
Режим безопасности: restrict
o
Aging time: 60 мин.
o
Aging type: неактивный
c.
Verify port security on S1 F0/6.

d.
Включите безопасность порта для F0 / 18 на S2. Настройте каждый активный порт доступа таким
образом, чтобы он автоматически добавлял адреса МАС, изученные на этом порту, в текущую
конфигурацию.
e.
Настройте следующие параметры безопасности порта на S2 F / 18:
o
Максимальное количество записей MAC-адресов: 2
o
Тип безопасности: Protect
o
Aging time: 60 мин.
f.
Проверка функции безопасности портов на S2 F0/18.

Шаг 5 Реализовать безопасность DHCP snooping.
a.
На S2 включите DHCP snooping и настройте DHCP snooping во VLAN 10
b.
Настройте магистральные порты на S2 как доверенные порты.
c.
Ограничьте ненадежный порт Fa0/18 на S2 пятью DHCP-пакетами в секунду.
d.
Проверка DHCP Snooping на

e.
В командной строке на PC-B освободите, а затем обновите IP-адрес.
C:\Users\Student> ipconfig /release
C:\Users\Student> ipconfig /renew
f.
Проверьте привязку отслеживания DHCP с помощью команды show ip dhcp snooping binding.
S2# show ip dhcp snooping binding
MacIp адресAddress Lease(sec) Type VLAN Interface
------------------ --------------- ---------- ------------- ---- --------------------
00:50:56:90:D0:8E 192.168.10.11 86213 dhcp-snooping 10 FastEthernet0/18
Total number of bindings: 1
Шаг 6 Реализация PortFast и BPDU Guard
a.
Настройте PortFast на всех портах доступа, которые используются на обоих коммутаторах.
b.
Включите защиту BPDU на портах доступа VLAN 10 S1 и S2, подключенных к PC-A и PC-B.
c.
Убедитесь, что защита BPDU и PortFast включены на соответствующих портах.
