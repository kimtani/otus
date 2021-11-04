Настройка протокола OSPFv2 для одной области

Топология

![](http://joxi.ru/VrwDB9JUjE9Pgm.jpg)

Таблица адресации

 | Устройство | Интерфейс | IP- адрес | Маска подсети | 
 | ------------- |:------------------:| -----:|-----:|
 | R1 | G0/0/1 |10.53.0.1  | 255.255.255.0 |
 |  |Loopback 1| 172.16.1.1|255.255.255.0|
 | R2|G0/0/1| 10.23.0.2|255.255.255.0||
 |  |Loopback 1|192.168.1.1 |255.255.255.0|
 
 
Цели

Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Настройка и проверка базовой работы протокола OSPFv2 для одной области

Часть3. Оптимизация ипроверка конфигурации OSPFv2 для одной области

Инструкции

Часть 1. Создание сети и настройка основных параметров устройства

 Шаг 1. Создать сеть согласно топологии
 
 Шаг 2 Произведите базовую настройку маршрутизаторов
 
a. Назначьте маршрутизатору имя устройства

b. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов

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
r1(config)#exit
r1#
%SYS-5-CONFIG_I: Configured from console by console

r1#copy run star
Destination filename [startup-config]? 
Building configuration...
[OK]

Switch>en 
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname s1
s1(config)#line console 0
s1(config-line)#exec-ti
s1(config-line)#exec-timeout 
% Incomplete command.
s1(config-line)#exec-timeout 0 0
s1(config-line)#logg sync
s1(config-line)#login local
s1(config-line)#password cisco
s1(config-line)#exit
s1(config)#line vty 0 4
s1(config-line)#password cisco
s1(config-line)#login
s1(config-line)#login local
s1(config-line)#exit
s1(config)#ser
s1(config)#service pas
s1(config)#service password-encryption 

----------------------------

r1(config)#int g0/0/1
r1(config-if)#ip ad 10.53.0.1 255.255.255.0
r1(config-if)#no sh

r1(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

r1(config-if)#exit
r1(config)#int lo
r1(config)#int loopback 1

r1(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

r1(config-if)#ip ad 172.16.1.1 255.255.255.0
r1(config-if)#exit

----------------------------

Enter configuration commands, one per line.  End with CNTL/Z.
r2(config)#int g0/0/1
r2(config-if)#ip ad 10.53.0.2 255.255.255.0
r2(config-if)#no sh

r2(config-if)#
%LINK-5-CHANGED: Interface GigabitEthernet0/0/1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0/1, changed state to up

r2(config-if)#exit
r2(config)#int lo
% Incomplete command.
r2(config)#int loo
r2(config)#int loopback 1

r2(config-if)#
%LINK-5-CHANGED: Interface Loopback1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Loopback1, changed state to up

r2(config-if)#ip ad 192.168.1.1 255.255.255.0
r2(config-if)#

