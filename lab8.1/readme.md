### Лабораторная работа - Реализация DHCPv4

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

d. Убедитесь, что статическая маршрутизация работает с помощью пинга до адреса G0/0/1 R2 от R1.

```
R1(config)#int g0/0/0
R1(config-if)#ip  add 10.0.0.1 255.255.255.252
R1(config-if)#no sh
R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

R1(config-if)#exit

R1(config)#ip route 0.0.0.0 0.0.0.0 10.0.0.2
R1(config)#do ping 192.168.1.97

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.97, timeout is 2 seconds:
.!!!!
Success rate is 80 percent (4/5), round-trip min/avg/max = 0/12/51 ms

R1(config)#do ping 192.168.1.97

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.97, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms

R1(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R1(config)#

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
S1(config)#ena
S1(config)#enable 
S1(config)#enable sec class
S1(config)#line con 0
S1(config-line)#pass
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#logg syn
S1(config-line)#logg synchronous 
S1(config-line)#exec
S1(config-line)#exec-timeout 0 0
S1(config-line)#exit
S1(config)#line vty 0 4
S1(config-line)#pass
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#ser
S1(config)#service pas
S1(config)#service password-encryption 
S1(config)#ba
S1(config)#banner mo
S1(config)#banner motd *Unauthorized access is strictly prohibited*
S1(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
S1(config)#ntp ser
S1(config)#ntp server ?
  Hostname or A.B.C.D  IP address of peer


S1(config)#ntp server 10.0.0.1

***

####  Шаг 7. Создайте сети VLAN на коммутаторе S1.

Примечание. S2 настроен только с базовыми настройками.

a. Создайте необходимые VLAN на коммутаторе 1 и присвойте им имена из приведенной выше таблицы.

b. Настройте и активируйте интерфейс управления на S1 (VLAN 200), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того установите шлюз по умолчанию на S1.

c. Настройте и активируйте интерфейс управления на S2 (VLAN 1), используя второй IP-адрес из подсети, рассчитанный ранее. Кроме того, установите шлюз по умолчанию на S2

d. Назначьте все неиспользуемые порты S1 VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их. На S2 административно деактивируйте все неиспользуемые порты.

Примечание. Команда interface range полезна для выполнения этой задачи с минимальным количеством команд.
