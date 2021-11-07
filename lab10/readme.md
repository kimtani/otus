### Лабораторная работа. Настройка протокола OSPFv2 для одной области

#### Топология

![](http://joxi.ru/VrwDB9JUjE9Pgm.jpg)

#### Таблица адресации

 | Устройство | Интерфейс | IP- адрес | Маска подсети | 
 | ------------- |:------------------:| -----:|-----:|
 | R1 | G0/0/1 |10.53.0.1  | 255.255.255.0 |
 |  |Loopback 1| 172.16.1.1|255.255.255.0|
 | R2|G0/0/1| 10.23.0.2|255.255.255.0||
 |  |Loopback 1|192.168.1.1 |255.255.255.0|
 
 
#### Цели

#### Часть 1. Создание сети и настройка основных параметров устройства

#### Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области

#### Часть3. Оптимизация ипроверка конфигурации OSPFv2 для одной области

----------------------------

#### Инструкции

#### Часть 1. Создание сети и настройка основных параметров устройства

#### Шаг 1. Создать сеть согласно топологии
 
#### Шаг 2 Произведите базовую настройку маршрутизаторов
 
a. Назначьте маршрутизатору имя устройства

b. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, 
как будто они являются именами узлов

c. Назначьте class в качестве зашифрованного пароля привелегированного режима EXEC

d. Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю

e. Назначьте cisco в качеcтве пароля VTY и включите вход в систему по паролю.

f. Зашифруйте открытые пароли.

g. Создайте баннер, который предупреждает о запрете несанкционного доступа.

h. Сохраните текущую конфигурацию в файл загрузочной конфигурации

a-h Для R1, R2 настроен по аналогии
 
```
Router(config)#hostname r1
r1(config)#no ip domain-lookup 
r1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
r1(config)#line console 0
r1(config-line)#exec-timeout 0 0
r(config-line)#logging synchronous 
r1(config-line)#password cisco
r1(config-line)#exit
r1(config)#enable secret class
r1(config)#line vty 0 4
r1(config-line)#login local
r1(config-line)#password cisco
r1(config-line)#exit
r1(config)#service password-encryption 
r1(config)#banner motd #Unauthorized access strictly prohibited#
r1(config)#exit
r1#
%SYS-5-CONFIG_I: Configured from console by console

r1#copy run star
Destination filename [startup-config]? 
Building configuration...
[OK]

```

#### Шаг 3. Настройте базовые параметры каждого коммутатора.

a. Присвойте коммутатору имя устройства

b. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов

c. Назначьте class в качестве зашифрованного пароля привелегированного режима EXEC

d. Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю

e. Назначьте cisco в качетсве пароля VTY и включите вход в систему по паролю.

f. Зашифруйте открытые пароли.

g. Создайте баннер, который предупреждает о запрете несанкционного доступа.

h. Сохранение текущей конфигурации в качестве начальной.

a-h Ддя S1, S2 настроен аналогично

```
Switch>en 
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname s1
s1(config)#line console 0
s1(config-line)#exec-timeout 0 0
s1(config-line)#logg sync
s1(config-line)#login local
s1(config-line)#password cisco
s1(config-line)#exit
s1(config)#line vty 0 4
s1(config-line)#password cisco
s1(config-line)#login
s1(config-line)#exit
s1(config)#service password-encryption 
s1(config)#banner motd #Unauthorized access strictly prohibited#
s1(config)#exit
s1#
%SYS-5-CONFIG_I: Configured from console by console

s1#copy run start
Destination filename [startup-config]? startup-config
Building configuration...
[OK]

```

----------------------------

#### Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области

#### Шаг 1. Настройте адреса интерфейса и базового OSPF на каждом маршрутизаторе.

a. Настройте адреса интерфейсов на каждом маршрутизаторе, как показано в таблице адресации
выше.

b. Перейдите в режим конфигурации маршрутизатора OSPF, используя идентификатор процесса 56

c. Настройте статический идентификатор маршрутизатора для каждого маршрутизатора (1.1.1.1 для
R1, 2.2.2.2 для R2).

d. Настройте инструкцию сети для сети между R1 и R2, поместив ее в область 0

e. Только на R2 добавьте конфигурацию, необходимую для объявления сети Loopback 1 в область
OSPF 0

a-d для r1

```
r1(config)#int g0/0/1
r1(config-if)#ip ad 10.53.0.1 255.255.255.0
r1(config-if)#no sh

r1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

r1(config-if)#exit
r1(config)#int loopback 1

r1(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

r1(config-if)#ip ad 172.16.1.1 255.255.255.0
r1(config-if)#exit
r1(config)#router ospf 56
r1(config-router)#router-id 1.1.1.1
r1(config-router)#int g0/0/1
r1(config-if)#ip ospf 56 are 0
r1(config-if)#
00:57:07: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

r1(config-if)#exit



```

a-e для r2

```
Enter configuration commands, one per line.  End with CNTL/Z.
r2(config)#int g0/0/1
r2(config-if)#ip ad 10.53.0.2 255.255.255.0
r2(config-if)#no sh

r2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

r2(config-if)#exit
r2(config)#int loopback 1

r2(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

r2(config-if)#ip ad 192.168.1.1 255.255.255.0
r2(config-if)#exit
r2(config)#router ospf 56
r2(config-router)#router-id 2.2.2.2
r2(config-router)#exit
r2(config)#int g0/0/1
r2(config-if)#ip ospf 56 ar 0
r2(config-if)#
00:16:28: %OSPF-5-ADJCHG: Process 56, Nbr 1.1.1.1 on GigabitEthernet0/0/1 from LOADING to FULL, Loading Done

r2(config-if)#exit
r2(config)#int loo
r2(config)#int loopback 1
r2(config-if)#ip os 56 ar 0
r2(config-if)#exit
r2(config)#


```

f. Убедитесь, что OSPFv2 работает между маршрутизаторами. Выполните команду, чтобы
убедиться, что R1 и R2 сформировали смежность.

Вопрос:
Какой маршрутизатор является DR? Какой маршрутизатор является BDR? Каковы критерии
отбора?

g. На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1
присутствует в таблице маршрутизации. Обратите внимание, что поведение OSPF по умолчанию
заключается в объявлении интерфейса обратной связи в качестве маршрута узла с
использованием 32-битной маски.

h. Запустите Ping до адреса интерфейса R2 Loopback 1 из R1. Выполнение команды ping должно
быть успешным.

f-h

```

r1#sh ip ospf nei


Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           1   FULL/BDR        00:00:30    10.53.0.2       GigabitEthernet0/0/1
r1#sh ip route os
     192.168.1.0/32 is subnetted, 1 subnets
O       192.168.1.1 [110/2] via 10.53.0.2, 00:04:32, GigabitEthernet0/0/1

r1#ping 192.168.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/2/13 ms

```
 - Маршрутизатор r1 является DR, r2 - BDR. В данном случае критерием отбора является номер идентификатора маршрутизатора
 
 Часть3. Оптимизация и проверка кофигурации OSPFv2 для одной области
 
a. На R1 настройте приоритет OSPF интерфейса G0/0/1 на 50, чтобы убедиться, что R1 является
назначенным маршрутизатором.

```
r1(config)int g0/0/1
r1(config-if)#ip ospf priority 50

```

b. Настройте таймеры OSPF на G0/0/1 каждого маршрутизатора для таймера приветствия,
составляющего 30 секунд.

для r1

```
r1(config)#int g0/0/1
r1(config-if)#ip os hello-interval 30
00:51:21: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Dead timer expired

00:51:21: %OSPF-5-ADJCHG: Process 56, Nbr 2.2.2.2 on GigabitEthernet0/0/1 from FULL to DOWN, Neighbor Down: Interface down or detached

r1(config-if)#ip os dead-interval 120
r1(config-if)#exit

```

для r2

```
r2(config)#int g0/0/1
r2(config-if)#ip os hello-interval 30
r2(config-if)#ip os dead-interval 120

```

c. На R1 настройте статический маршрут по умолчанию, который использует интерфейс Loopback 1 в
качестве интерфейса выхода. Затем распространите маршрут по умолчанию в OSPF. Обратите
внимание на сообщение консоли после установки маршрута по умолчанию.


```
r1(config)#ip route 0.0.0.0 0.0.0.0 loop
r1(config)#ip route 0.0.0.0 0.0.0.0 loopback 1
%Default route without gateway, if not a point-to-point interface, may impact performance
r1(config)#router ospf 56
r1(config-router)#default ?
  originate  Distribute a default route
r1(config-router)#default originate
r1(config-router)#exit

```

d. добавьте конфигурацию, необходимую для OSPF для обработки R2 Loopback 1 как сети точка-
точка. Это приводит к тому, что OSPF объявляет Loopback 1 использует маску подсети
интерфейса.

```
r2(config)#int lo 1
r2(config-if)#ip ospf network ?
  point-to-point  Specify OSPF point-to-point network
r2(config-if)#ip ospf network po
r2(config-if)#ip ospf network point-to-point 
r2(config-if)#do sh ip ospf int lo 1

Loopback1 is up, line protocol is up
  Internet address is 192.168.1.1/24, Area 0
  Process ID 56, Router ID 2.2.2.2, Network Type POINT-TO-POINT, Cost: 1
  Transmit Delay is 1 sec, State POINT-TO-POINT,
  Timer intervals configured, Hello 30, Dead 120, Wait 120, Retransmit 5
  Index 2/2, flood queue length 0
  Next 0x0(0)/0x0(0)
  Last flood scan length is 1, maximum is 1
  Last flood scan time is 0 msec, maximum is 0 msec
  Suppress hello for 0 neighbor(s)
r2(config-if)#exit

```


e. Только на R2 добавьте конфигурацию, необходимую для предотвращения отправки объявлений
OSPF в сеть Loopback 1


```
r2(config)#router ospf 56
r2(config-router)#pas
r2(config-router)#passive-interface lo 1

```

f. Измените базовую пропускную способность для маршрутизаторов. После этой настройки
перезапустите OSPF с помощью команды clear ip ospf process . Обратите внимание на
сообщение консоли после установки новой опорной полосы пропускания.

для r1, аналогично настроен r2

```
r1(config-router)#int g0/0/1
r1(config-if)#ba 60
r1(config-if)#exit
r1(config)#exit
r1#
%SYS-5-CONFIG_I: Configured from console by console

r1#clear ip os pro
Reset ALL OSPF processes? [no]: yes

```


#### Шаг 2 Убедитесь, что оптимизация OSPFv2 реализовалась.

a. Выполните команду show ip ospf interface g0/0/1 на R1 и убедитесь, что приоритет интерфейса
установлен равным 50, а временные интервалы — Hello 30, Dead 120, а тип сети по умолчанию —
Broadcast

![](http://joxi.ru/LmGKL0oTgPVyXm.jpg)

b. На R1 выполните команду show ip route ospf, чтобы убедиться, что сеть R2 Loopback1
присутствует в таблице маршрутизации. Обратите внимание на разницу в метрике между этим
выходным и предыдущим выходным. Также обратите внимание, что маска теперь составляет 24
бита, в отличие от 32 битов, ранее объявленных.

![](http://joxi.ru/12MnONoHwPZwdA.jpg)

c. Введите команду show ip route ospf на маршрутизаторе R2. Единственная информация о
маршруте OSPF должна быть распространяемый по умолчанию маршрут R1.


d.Запустите Ping до адреса интерфейса R1 Loopback 1 из R2. Выполнение команды ping должно
быть успешным.

![](http://joxi.ru/a2XgqGoUlBqp3m.jpg)


Вопрос:
Почему стоимость OSPF для маршрута по умолчанию отличается от стоимости OSPF в R1 для
сети 192.168.1.0/24?




