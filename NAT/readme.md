
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

Настройки для R1

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

Настройки для R2

```

Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#host R2
R2(config)#no ip domain-name 
R2(config)#enable secret class
R2(config)#line con 0
R2(config-line)#pass cisco
R2(config-line)#login
R2(config-line)#logg sync
R2(config-line)#exec-timeout 0 0
R2(config-line)#exit
R2(config)#line vty 0 15
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#service password-encryption 
R2(config)#banner motd *Unauthorized access is strictly prohibited*
R2(config)#int g0/0/0
R2(config-if)#no sh

R2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/0, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/0, changed state to up

R2(config-if)#ip add 209.165.200.225 255.255.255.248
R2(config-if)#exit
R2(config)#int lo1

R2(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

R2(config-if)#ip add 209.165.200.1 255.255.255.224
R2(config-if)#exit
R2(config)#ip route 0.0.0.0 0.0.0.0 209.165.200.230
R2(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
R2(config)#

```

#### Шаг 3. Настройте базовые параметры каждого коммутатора.

Откройте окно конфигурации

a. Присвойте коммутатору имя устройства.

b. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов.

c. Назначьте class в качестве зашифрованного пароля привилегированного режима EXEC.

d. Назначьте cisco в качестве пароля консоли и включите вход в систему по паролю.

e. Назначьте cisco в качестве пароля VTY и включите вход в систему по паролю.

f. Зашифруйте открытые пароли.

g. Создайте баннер с предупреждением о запрете несанкционированного доступа к устройству.

h. Выключите все интерфейсы, которые не будут использоваться.

i. Настройте IP-адресации интерфейса, как указано в таблице выше.

j. Сохраните текущую конфигурацию в файл загрузочной конфигурации.

Настройки для коммутатора S1, S2 настроен аналогично

```
Switch>
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname S1
S1(config)#no ip domain-name
S1(config)#ena
S1(config)#enable secret class
S1(config)#ser
S1(config)#service pas
S1(config)#service password-encryption 
S1(config)#banner motd *Unauthorized access is strictly prohibited*
S1(config)#line con 0
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#logg sync
S1(config-line)#exec-timeout 0 0
S1(config-line)#exit
S1(config)#line vty 0 15
S1(config-line)#pass cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#int ra f0/2-4, f0/7-24, g0/1-2
S1(config-if-range)#sh

%LINK-5-CHANGED: Interface FastEthernet0/2, changed state to administratively down
.
.
%LINK-5-CHANGED: Interface GigabitEthernet0/2, changed state to administratively down
S1(config-if-range)#exit

S1(config)#int vlan 1
S1(config-if)#ip add 192.168.1.11 255.255.255.0
S1(config-if)#exit
S1(config)#do copy run st
Destination filename [startup-config]? 
Building configuration...
[OK]
S1(config)#

```

#### Часть 2. Настройка и проверка NAT для IPv4.

В части 2 необходимо настроить и проверить NAT для IPv4.

#### Шаг 1. Настройте NAT на R1, используя пул из трех адресов 209.165.200.226-209.165.200.228.

Откройте окно конфигурации

a. Настройте простой список доступа, который определяет, какие хосты будут разрешены для трансляции. В этом случае все устройства в локальной сети R1 имеют право на трансляцию.

R1(config)# access-list 1 permit 192.168.1.0 0.0.0.255

b. Создайте пул NAT и укажите ему имя и диапазон используемых адресов.

R1(config)# ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.228 netmask 255.255.255.248

Примечание. Параметр маски сети не является разделителем IP-адресов. Это должна быть правильная маска подсети для назначенных адресов, даже если вы используете не все адреса подсети в пуле.

c. Настройте перевод, связывая ACL и пул с процессом преобразования.

R1(config)# ip nat inside source list 1 pool PUBLIC_ACCESS

Примечание: Три очень важных момента. Во-первых, слово «inside» имеет решающее значение для работы такого рода NAT. Если вы опустить его, NAT не будет работать. Во-вторых, номер списка — это номер ACL, настроенный на предыдущем шаге. В-третьих, имя пула чувствительно к регистру.

d. Задайте внутренний (inside) интерфейс.

R1(config)# interface g0/0/1
R1(config-if)# ip nat inside

e. Определите внешний (outside) интерфейс.

R1(config)# interface g0/0/0

R1(config-if)# ip nat outside

```

R1(config)#ac
R1(config)#access-list 1 per
R1(config)#access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)#ip nat pool PUBLIC_ACCESS 209.165.200.226 209.165.200.225 netmask 255.255.255.248
R1(config)#ip nat inside source list 1 pool PUBLIC_ACCESS
R1(config)#int g0/0/1
R1(config-if)#ip nat inside 
R1(config-if)#exit
R1(config)#int g0/0/0
R1(config-if)#ip nat outside 
R1(config-if)#

```

Шаг 2. Проверьте и проверьте конфигурацию.

a. С PC-B, запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните процесc поиска и устранения неполадок. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

![](http://joxi.ru/8AnYJ9nUoZ4W92.jpg)

![](http://joxi.ru/zANXnjoT8pnyPm.jpg)



Вопросы:

Во что был транслирован внутренний локальный адрес PC-B?

Введите ваш ответ здесь.

209.165.200.226

Какой тип адреса NAT является переведенным адресом?

Не понимаю вопрос

b. С PC-A, запустите эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.


![](http://joxi.ru/a2XgqGoUln3Vbm.jpg)


c. Обратите внимание, что предыдущая трансляция для PC-B все еще находится в таблице. Из S1, эхо-запрос интерфейса Lo1 (209.165.200.1) на R2. Если эхо-запрос не прошел, выполните отладку. На R1 отобразите таблицу NAT на R1 с помощью команды show ip nat translations.

![](http://joxi.ru/GrqDb9kUReDKVA.jpg)


d. Теперь запускаем пинг R2 Lo1 из S2. На этот раз перевод завершается неудачей, и вы получаете эти сообщения (или аналогичные) на консоли R1:

Sep 23 15:43:55.562: %IOSXE-6-PLATFORM: R0/0: cpp_cp: QFP:0.0 Thread:000 TS:00000001473688385900 %NAT-6-ADDR_ALLOC_FAILURE: Address allocation failed; pool 1 may be exhausted [2]

У меня пинг проходил успешно (a). 

![](http://joxi.ru/GrqDb9kUReDJVA.jpg)

Однако, если на обоих ПК запустить бесконечный пинг и на S1 одновременно отправлять пинги на Lo1 R2,  то действительно, ответ приходить не будет(b).  

 ![](http://joxi.ru/ZrJRa0oCbV80xr.jpg)
 
 Судя по таблице NAT translations, закончились адреса из пула для трансляции
 
![](http://joxi.ru/1A503akTzqYWz2.jpg)
![](http://joxi.ru/vAWqJpoUBvZDRm.jpg)

e. Это ожидаемый результат, потому что выделено только 3 адреса, и мы попытались ping Lo1 с четырех устройств. Напомним, что NAT — это трансляция «один-в-один». Как много выделено трансляций? Введите команду show ip nat translations verbose , и вы увидите, что ответ будет 24 часа.

R1# show ip nat translations verbose

Pro Inside global Inside local Outside local Outside global

--- 209.165.200.226 192.168.1.3 --- ---

create: 09/23/19 15:35:27, use: 09/23/19 15:35:27, timeout: 23:56:42

Map-Id(In): 1

<output omitted>

f. Учитывая, что пул ограничен тремя адресами, NAT для пула адресов недостаточно для нашего приложения. Очистите преобразование NAT и статистику, и мы перейдем к PAT.

R1# clear ip nat translations *

R1# clear ip nat statistics
![]()

![]()
![]()
![]()
![]()
![]()
![]()

