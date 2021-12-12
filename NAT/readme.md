
## Лабораторная работа - Настройка NAT для IPv4

#### Топология

| Устройство | Интерфейс| IP-адрес | Маска подсети | 
| ------------- |:------------------:| -----:|-----:|
| R1 | G0/0/0 | 209.165.200.230 | 255.255.255.248 | 
|  | G0/0/1 |192.168.1.1   | 255.255.255.0 | 
| R2 |G0/0/0  | 209.165.200.225 |  255.255.255.248 | 
|  | Lo1| 209.165.200.1 |  255.255.255.224 | 
| S1 | VLAN 1 | 192.168.1.11  | 255.255.255.0 | 
|  S2 | VLAN 2 | 192.168.1.12 | 255.255.255.0 | 
| PCA |NIC | 192.168.1.2 | 255.255.255.0 | 
|  PCB|NIC  | 192.168.1.3 | 255.255.255.0 | 

#### Цели

#### Часть 1. Создание сети и настройка основных параметров устройства

#### Часть 2. Настройка и проверка NAT для IPv4

#### Часть 3. Настройка и проверка PAT для IPv4

#### Часть 4. Настройка и проверка статического NAT для IPv4.

#### Инструкции

#### Часть 1. Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые параметры для узлов ПК и коммутаторов.

#### Шаг 1. Подключите кабели сети согласно приведенной топологии.

Подключите устройства в соответствии с топологией и подсоедините соответствующие кабели.

#### Шаг 2. Произведите базовую настройку маршрутизаторов.

Откройте окно конфигурации

a. Назначьте маршрутизатору имя устройства.

b. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c. Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d. Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e. Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f. Зашифруйте открытые пароли.

g. Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h. Настройте IP-адресации интерфейса, как указано в таблице выше.

i. Настройте маршрут по умолчанию. от R2 до R1.

j. Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```

Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#host R1
R1(config)#enable secret class
R1(config)#line con 0
R1(config-line)#pass cisco
R1(config-line)#login
R1(config-line)#logg syn
R1(config-line)#exec-timeout 0 0
R1(config-line)#exit
R1(config)#no ip domain-name
R1(config)#line vty 0 15
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#service pas
R1(config)#service password-encryption 
R1(config)#banner motd *Unautorized access is strictly prohibited*
R1(config)#int g0/0/1
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

R1(config-if)#ip add 192.168.1.1 255.255.255.0
R1(config-if)#int g0/0/0
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

R1(config-if)#ip add 209.165.200.230 255.255.255.248
R1(config-if)#exit
R1(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R1(config)#

```

![]()
![]()
![]()![]()
![]()
![]()
![]()
![]()
![]()

