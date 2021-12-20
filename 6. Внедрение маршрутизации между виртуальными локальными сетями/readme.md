## Лабораторная работа - Внедрение маршрутизации между виртуальными локальными сетями

#### Часть 1. Топология

![](http://joxi.ru/MAjDy9aU1O7K5m.jpg)

#### Таблица адресации

 | Устройство | Интерфейс | IP- адрес | Маска подсети | Шлюз по умолчанию |
 | ------------- |:------------------:| -----:|-----:|-----:|
 | R1 | G0/0/1.10 | 192.168.10.1 | 255.255.255.0 | |
 |  |G0/0/1.20| 192.168.20.1|255.255.255.0||
 | |G0/0/1.30| 192.168.30.1|255.255.255.0||
 |  |G0/0/1.1000| |||
 | S1 | VLAN 10| 192.168.10.11 | 255.255.255.0 |192.168.10.1|
 | S2 | VLAN 10| 192.168.10.12 | 255.255.255.0 |192.168.10.1|
 | PC-A | NIC | 192.168.20.3| 255.255.255.0 |192.168.20.1 |
 | PC-B | NIC | 192.168.30.33| 255.255.255.0 |192.168.30.1 |
 

#### Таблица VLAN

 | VLAN | Имя  | Назначенный интерфейс |
 | ------------- |:------------------:| -----:|
 | 10 | Управление | S1: VLAN 10|
 |||S2: VLAN 10|
 | 20 | Sales| S1: F0/6|
 | 30 | Operations | S2: F0/18 |
 | 999 |Parking_Lot| S1:F0/2-4,F0/7-24, G0/0/1-2|
  |||S2: F0/2-17,F0/19-24, G0/0/1-2|
 | 1000 |Собственная| - |

#### Задачи 

##### Часть 1. Создание сети и настройка основных параметров устройства

##### Часть 2. Создание сетей VLAN и название портов коммутатора

##### Часть 3. Настройка транка 802.1Q между коммутаторами

##### Часть 4. Настройка маршрутизации между сетями VLAN

##### Часть 5. ПРоверка, что маршрутизация между VLAN работает


#### Интсрукции

##### Часть 1. Создание сети и настройка основных параметров устройства

#### Шаг 1. Создайте сеть согласно топологии

a. Подключитесь к маршрутизатору с помощью консоли и активируйте привелегированный режим EXEC

b. Войдите в режим конфигурации

c. Назначьте маршрутизатору имя устройства

c. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов

d. Назначьте *class* в качестве зашифрованного пароля привелегированного режима EXEC

e. Назначьте *cisco* в качестве пароля консоли и включите вход в систему по паролю

f. Назначьте *cisco* в качетсве пароля VTY  и включите вход в систему по паролю.

g. Зашифруйте открытые пароли.

h. Создайте баннер, который предупреждает о запрете несанкционного доступа.

j. Сохраните текущую конфигурацию в файл загрузочной конфигурации

k. Настройте на маршрутизаторе время

a-k


```
Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#no ip domain-lookup
R1(config)#enable secret class
R1(config)#line console 0
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#line vty 0 15
R1(config-line)#password cisco
R1(config-line)#login
R1(config-line)#exit
R1(config)#service password-encryption 
R1(config)#banner motd #Unauthorized access strictly prohibited#
R1(config)#exit
R1#
%SYS-5-CONFIG_I: Configured from console by console

R1#copy run startup-config 
Destination filename [startup-config]? startup-config
Building configuration...
[OK]
R1#clock set 11:12:00 20 sep 2021
R1#
```



#### Шаг 3. Настройте базовые параметры каждого коммутатора

a. Подключитесь к коммутатору с помощью консольного подключения и активируйте привелегированный режим EXEC

b. Присвойте коммутатору имя устройства

c. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов

d. Назначьте *class* в качестве зашифрованного пароля привелегированного режима EXEC

e. Назначьте *cisco* в качестве пароля консоли и включите вход в систему по паролю

f. Назначьте *cisco* в качетсве пароля VTY  и включите вход в систему по паролю.

g. Зашифруйте открытые пароли.

h. Создайте баннер, который предупреждает о запрете несанкционного доступа.

i. Настройте на коммутаторе врмея

j. Сохранение текущей конфигурации в качестве начальной.

a-j

S1

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
S1(config)#exit
S1#clock set 11:26:00 20 sep 2021
S1#copy run start
Destination filename [startup-config]? startup-config
Building configuration...
[OK]
S1#
```

S2

```
Switch>en
Switch#clock set 11:29:00 20 sep 2021
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname S2
S2(config)#enable sec class
S2(config)#line console 0
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#line vty 0 15
S2(config-line)#pass cisco
S2(config-line)#login
S2(config-line)#exit
S2(config)#serv password-encryption 
S2(config)#banner motd #Unauthorized access strictly prohibited#
S2(config)#exit
S2#
%SYS-5-CONFIG_I: Configured from console by console

S2#copy run start
Destination filename [startup-config]? startup-config
Building configuration...
[OK]
	
S2#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S2(config)#no ip domain-lookup 
S2(config)#exit
S2#
%SYS-5-CONFIG_I: Configured from console by console
```


#### Шаг 4. Настройте узлы ПК

PC-A

![](http://dl4.joxi.net/drive/2021/09/20/0050/1314/3282210/10/bb86b20140.jpg)


PC-B

![](http://dl4.joxi.net/drive/2021/09/20/0050/1314/3282210/10/bc181e6fb3.jpg)

------

### Часть 2. Создание сетей VLAN  и назначение портов коммутатора

#### Шаг 1.

a. Создайте и назовите необходимые VLAN  на каждом коммутаторе из таблицы выше

b. Настройте интерфейс управления и шлюз по умолчанию на каждом коммутаторе, используя информацию об IP-адресе втаблице адресации.

с. Назначьте все неиспользуемые порты коммутатора VLAN Parking_Lot, настройте их для статического режима доступа и административно деактивируйте их

a-c

S1

```
s1(config)#vlan 10
s1(config-vlan)#name MANAGEMENT
s1(config-vlan)#exit
s1(config)#vlan 20
s1(config-vlan)#name SALES
s1(config-vlan)#exit
s1(config)#vlan 30
s1(config-vlan)#name OPERATIONS
s1(config-vlan)#exit
s1(config)#vlan 999
s1(config-vlan)#name Parking_Lot
s1(config-vlan)#exit
s1(config)#vlan 1000
s1(config-vlan)#name NATIVE
s1(config-vlan)#exit
s1(config)#int vlan 10
s1(config-if)#
%LINK-5-CHANGED: Interface Vlan10, changed state to up

s1(config-if)#ip ad 192.168.10.11 255.255.255.0
s1(config-if)#exit
s1(config)#ip default-gateway 192.168.10.1

s1(config)#int f0/6
s1(config-if)#sw mo ac
s1(config-if)#sw ac vlan 20
s1(config-if)#exit
s1(config)#int ra f0/2-4
s1(config-if-range)#sw mo ac
s1(config-if-range)#sw ac vlan 999
s1(config-if-range)#exit
s1(config)#int ra f0/7-24
s1(config-if-range)#sw mo ac
s1(config-if-range)#sw ac vlan 999
s1(config-if-range)#exit
s1(config)#int ra g0/1-2
s1(config-if-range)#sw mo ac
s1(config-if-range)#sw ac vlan 999

```
S 2 созданы vlan по аналогии

#### Шаг 2. Назначьте сети VLAN соответствующим интерфейсам коммутатора 

a. Назначьте использоуемые порты соответсвующей VLAN, и настройте их для режима статического доступа

b. Убедитесь, что VLAN назначены правильные интерфейсы

a.Настройки для S1; аналогично для s2

```
s1(config)#int f0/6
s1(config-if)#sw mo ac
s1(config-if)#sw ac vlan 20

```

b.

![](http://joxi.ru/MAjDy9aU1llE8m.jpg)

![](http://joxi.ru/EA4GRZxCv11j52.jpg)
  -------
  
### Часть 3. Конфигурация магистрального канала стандарта 802.1Q между коммутаторами

#### Шаг 1. Вручную настройте магистральный интерфейс F0/1 на коммутаторе S1 и S2

a. Настройка статического транкинга на интерфейсе F0/1 для обоих коммутаторов.

b. Установите native VLAN 1000 на обоих коммутаторах.

c. Укажите,что VLAN 10, 20, 30 и 1000 могут проходить по транку

d. Проверьте транки, native VLAN и разрешенные VLAN через транк.

a-d для S1, аналогично настроен S2

```
s1(config)#int f0/1
s1(config-if)#sw trunk native vlan 1000
s1(config-if)#sw mo tru
s1(config-if)#sw tru al vlan 10,20,30,1000

```

#### Шаг 2. Вручную настройте магистральный интерфейс F0/5 на коммутаторе S1

a. Настройте интерфейс S1 F0/5 с теми же параметрами транка, что и F0/1. Это транк до маршрутизатора.

b. Сохраните текущуюконфигурацию в файл загрузочной конфигурации.

```
s1(config)#int f0/5
s1(config-if)#sw trunk native vlan 1000
s1(config-if)#sw mo tru
s1(config-if)#sw tru al vlan 10,20,30,1000

s1(config)#exit 
s1#
%SYS-5-CONFIG_I: Configured from console by console

s1#copy run star
Destination filename [startup-config]? 
Building configuration...

```


с. Проверка транкинга.

Что произойдет, если G0/0/1  на R1 будет отключен?

Трафик не будет поступать на маршрутизатор

### Часть 4. Настройка маршрутизации между сетями VLAN

#### Шаг 1. Настройте маршрутизатор

a. При необходимости активируйте интерфейс G0/0/1 на маршрутизаторе

b. Настройте подинтерфейсы для каждой VLAN, как указано в таблице IP-адресации. Все подинтерфейсы используют инкапсуляцию 802.1Q. Убедитесь, что подинтерфесу для native VLAN не назначен IP-адрес. Включите описание для каждого подинтерфейса.

с. Убедитесь, что вспомогательные интерфейсы работают

a-c

```
R1(config)#int g0/0/1
R1(config-if)#no sh

R1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up
	
R1(config)#int g0/0/1.20
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.20, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.20, changed state to up

R1(config-subif)#enc dot1q 20
R1(config-subif)#ip ad 192.168.20.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.10
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.10, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.10, changed state to up

R1(config)#int g0/0/1.10
R1(config-subif)#enca dot1q 10
R1(config-subif)#ip ad 192.168.10.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.30
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.30, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.30, changed state to up

R1(config-subif)#enc dot1q 30
R1(config-subif)#ip ad 192.168.30.1 255.255.255.0
R1(config-subif)#exit
R1(config)#int g0/0/1.1000
R1(config-subif)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1.1000, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1.1000, changed state to up

R1(config-subif)#enc dot1q 1000
R1(config-subif)#exit
R1(config)#exit
R1#
%SYS-5-CONFIG_I: Configured from console by console

```


### Часть 5. Проверьте, работает ли маршрутизация между VLAN

#### Шаг 1. Отправьте эхо-запрос с PC-A.Все должно быть успешно

a. Отправьте эхо-запрос с PC-A на шлюз по умолчанию

![](http://joxi.ru/Dr8pXO7CJaa99r.jpg)

b. Отправьте эхо-запрос с PC-A на PC-B

![](http://joxi.ru/a2XgqGoUlBBzdm.jpg)

c. Отправьте команду ping с компьютера PC-A на коммутатор S2

![](http://joxi.ru/V2VJNgot8PPRg2.jpg)

#### Шаг 2. ПРойдите следующий тест с PC-B

В окне командной строки на PC-B выполните команду tracert на адрес PC-A

Какие промежуточные адреса отображаются в результатах?

![](http://joxi.ru/KAgDxXLUNWWZBr.jpg)




