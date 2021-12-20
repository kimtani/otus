## Настройка DHCPv6

### Лабораторная работа выполнена для курса [**Network Engineer. OTUS**](https://otus.ru/lessons/network-engineer-specialization/#rec313881739)

![](http://joxi.ru/BA0JLWltvngaPm.jpg)


#### Топология

![](http://joxi.ru/KAgDxXLUNdk3Br.jpg)

#### Таблица адресации

 | Устройство | Интерфейс | IP- адрес | 
 | ------------- |:------------------:| -----:|
 | R1 | G0/0/0 | 2001:db8:acad:2::1/64 | 
 |  ||fe80::1|
 |  |G0/0/1| 2001:db8:acad:1::1/64|
 |  ||fe80::1|
 | R2 | G0/0/0 | 2001:db8:acad:2::2/64 | 
 |  ||fe80::1|
 |  |G0/0/1| 2001:db8:acad:3::1/64|
 |  ||fe80::1|
 | PC-A | NIC | DHCP| 
 | PC-B | NIC | DHCP| 
 
 #### Задачи:
**Часть 1. Создание сети и настройка основных параметров устройства**
#### Часть 2. Проверка назначения адреса SLAAC от R1
#### Часть 3. Настройка и проверка сервера DHCPv6 без гражданства на R1
#### Часть 4. Настройка и проверка состояния DHCPv6 сервера на R1
#### Часть 5. Настройка и проверка DHCPv6 Relay на R2

-----------------

**Инструкции**

#### Часть 1. Создание сети и настройка основных параметров устройства

В первой части лабораторной работы вам предстоит создать топологию сети и настроить базовые
параметры для узлов ПК и коммутаторов.

#### Шаг 1. Создайте сеть согласно топологии.

Подключите устройства, как показано в топологии, и подсоедините необходимые кабели. Откройте окно конфигурации

#### Шаг 2. Настройте базовые параметры каждого коммутатора. 

a. Присвойте коммутатору имя устройства.

b. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать
введенные команды таким образом, как будто они являются именами узлов.

c. Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d. Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e. Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f. Зашифруйте открытые пароли.

g. Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h. Отключите все неиспользуемые порты.

i. Сохраните текущую конфигурацию в файл загрузочной конфигурации.
Закройте окно настройки.

a-i для S1 и S2

```
Press RETURN to get started!


Switch>
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#enable secret class
Switch(config)#exit
Switch#
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#line console 0
Switch(config-line)#password cisco
Switch(config-line)#login
Switch(config-line)#exit
Switch(config)#line vty 0 15
Switch(config-line)#password cisco
Switch(config-line)#login
Switch(config-line)#exit
Switch(config)#ser password-encryption 
Switch(config)#banner motd #Unauthorized access strictly prohibited#
Switch(config)#exit
Switch#
%SYS-5-CONFIG_I: Configured from console by console

Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname S1
S1(config)#no ip domain-lookup
S1(config)exit
S1#
S1#copy run start
Destination filename [startup-config]? startup-config
Building configuration...
[OK]

```
#### Шаг 3. Произведите базовую настройку маршрутизаторов.

a. Назначьте маршрутизатору имя устройства.

b. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать
введенные команды таким образом, как будто они являются именами узлов.

c. Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d. Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e. Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f. Зашифруйте открытые пароли.

g. Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h. Активация IPv6-маршрутизации

i. Сохраните текущую конфигурацию в файл загрузочной конфигурации.

a-i для R1, R2

```
Router>en
Router#conf t
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#line con 0
Router(config-line)#password cisco
Router(config-line)#login
Router(config-line)#logg sync
Router(config-line)#exec
Router(config-line)#exec-timeout 0 0
Router(config-line)#exit
Router(config)#hos
Router(config)#hostname R1
R1(config)#ser
R1(config)#service pa
R1(config)#service password-encryption 
R1(config)#ipv6 uni
R1(config)#ipv6 unicast-routing 
R1(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...


R1(config)#ipv6 route 2001:db8:acad:3::1/64 2001:db8:acad:2::2
R1(config)#exit
R1#
%SYS-5-CONFIG_I: Configured from console by console

```

#### Шаг 4. Настройка интерфейсов и маршрутизации для обоих маршрутизаторов.

a. Настройте интерфейсы G0/0/0 и G0/1 на R1 и R2 с адресами IPv6, указанными в таблице выше.

b. Настройте маршрут по умолчанию на каждом маршрутизаторе, который указывает на IP-адрес
G0/0/0 на другом маршрутизаторе.

c. Убедитесь, что маршрутизация работает с помощью пинга адреса G0/0/1 R2 из R1

d. Сохраните текущую конфигурацию в файл загрузочной конфигурации.

a-d для R1, R2 настроен аналогично

```

R1(config)#int g0/0/0
R1(config-if)no sh
R1(config-if)#ipv6 add 2001:db8:acad:2::1/64
R1(config-if)#ipv6 add fe80::1 lin
R1(config-if)#ipv6 add fe80::2 link-local 
R1(config-if)#int g0/0/1
R1(config-if)no sh
R1(config-if)#ipv6 add 2001:db8:acad:1::1/64
R1(config-if)#ipv6 add fe80::1 li
R1(config-if)#ipv6 add fe80::1 link-local 
R1(config-if)#exit
R1(config)#ipv6 route ::/0 2001:db8:acad:2::2
R1(config)#exit
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1(config)#do ping 2001:db8:acad:3::1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 2001:db8:acad:3::1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms

R1(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R1(config)#

```
### Часть 2. Проверка назначения адреса SLAAC от R1

В части 2 вы убедитесь, что узел PC-A получает адрес IPv6 с помощью метода SLAAC.
Включите PC-A и убедитесь, что сетевой адаптер настроен для автоматической настройки IPv6.
Через несколько минут результаты команды ipconfig должны показать, что PC-A присвоил себе адрес
из сети 2001:db8:1::/64.

C:\Users\Student> ipconfig

![](http://joxi.ru/D2Pkx0oCBV7KKm.jpg)

Откуда взялась часть адреса с идентификатором хоста?

### Часть 3. Настройка и проверка сервера DHCPv6 на R1


Шаг 1. Более подробно изучите конфигурацию PC-A.


a.	Выполните команду ipconfig /all на PC-A и посмотрите на результат.

![](http://joxi.ru/823MaKXCaW3dDr.jpg)

b.	Обратите внимание, что основной DNS-суффикс отсутствует. Также обратите внимание, что предоставленные адреса DNS-сервера являются адресами «локального сайта anycast», а не одноадресные адреса, как ожидалось.


#### Шаг 2. Настройте R1 для предоставления DHCPv6 без состояния для PC-A.

a.	Создайте пул DHCP IPv6 на R1 с именем R1-STATELESS. В составе этого пула назначьте адрес DNS-сервера как 2001:db8:acad: :1, а имя домена — как stateless.com.

b.	Настройте интерфейс G0/0/1 на R1, чтобы предоставить флаг конфигурации OTHER для локальной сети R1 и укажите только что созданный пул DHCP в качестве ресурса DHCP для этого интерфейса.

c.	Сохраните текущую конфигурацию в файл загрузочной конфигурации.


```
R1(config)#ipv6 dhcp pool R1-STATELESS
R1(config-dhcpv6)#dns-ser
R1(config-dhcpv6)#dns-server 2001:db8:acad::254
R1(config-dhcpv6)#domain-name STATELESS.com
R1(config-dhcpv6)#int g0/0/1
R1(config-if)#ipv6 nd other
R1(config-if)#ipv6 nd other-config-flag 
R1(config-if)#ipv6 dhcp server
R1(config-if)#ipv6 dhcp server R1-STATELESS
R1(config-if)#
R1(config-if)#^Z
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R1#

```

d.	Перезапустите PC-A.

e.	Проверьте вывод ipconfig /all и обратите внимание на изменения.


![](http://joxi.ru/LmGKL0oTg3DEom.jpg)

f. Тестирование подключения с помощью пинга IP-адреса интерфейса G0/1 R2

![](http://joxi.ru/GrqDb9kURax9pA.jpg)


### Часть 4. Настройка сервера DHCPv6 с сохранением состояния на R1


a.	Создайте пул DHCPv6 на R1 для сети 2001:db8:acad:3:aaa::/80. Это предоставит адреса локальной сети, подключенной к интерфейсу G0/0/1 на R2. В составе пула задайте DNS-сервер 2001:db8:acad: :254 и задайте доменное имя STATEFUL.com.


```
R1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
R1(config)#ipv6 dhcp pool R2-STATEFUL
R1(config-dhcpv6)#address prefix 2001:db8:acad:3:aaa::/80
R1(config-dhcpv6)#dns
R1(config-dhcpv6)#dns-server 2001:db8:acad::254
R1(config-dhcpv6)#domain
R1(config-dhcpv6)#domain-name STATEFUL.com
R1(config-dhcpv6)#int g0/0/0
R1(config-if)#ipv6 dhcp se
R1(config-if)#ipv6 dhcp server R2-STATEFUL
R1(config-if)#

```

### Часть 5. Настройка и проверка ретрансляции DHCPv6 на R2.


#### Шаг 1. Включите PC-B и проверьте адрес SLAAC, который он генерирует.


![](http://joxi.ru/823MaKXCaW3VPr.jpg)


#### Шаг 2. Настройте R2 в качестве агента DHCP-ретрансляции для локальной сети на G0/01

a.	Настройте команду ipv6 dhcp relay на интерфейсе R2 G0/0/1, указав адрес назначения интерфейса G0/0/0 на R1. 
Также настройте команду managed-config-flag .

```
R2(config)#int g0/0/1

R2(config-if)#ipv6 nd managed-config-flag 
R2(config-if)#ipv6 dhcp relay destination 2001:db8:acad:2::1 g0/0/0
R2(config-if)#^Z
R2#
%SYS-5-CONFIG_I: Configured from console by console

R2#copy run st
Destination filename [startup-config]? 
Building configuration...

``` 
![]()
![]()
![]()
