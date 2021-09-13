### Лабораторная работа. Доступ к сетевым устройствам по протоколу SSH

#### Топология

![](http://dl3.joxi.net/drive/2021/09/13/0050/1314/3282210/10/430a74592d.jpg)

#### Таблица адресации

 | Устройство | Интерфейс | IP- адрес | Маска подсети | Шлюз по умолчанию |
 | ------------- |:------------------:| -----:|-----:|-----:|
 | R1 | G0/0/1 | 192.168.1.1 | 255.255.255.0 | |
 | S1 | VLAN 1| 192.168.1.11 | 255.255.255.0 |192.168.1.1|
 | PC-A | NIC | 192.168.1.3| 255.255.255.0 |192.168.1.1 |

#### Задачи

##### Часть 1. Настройка основных параметров устройства

##### Часть 2. Настройка маршрутизатора для доступа по протоколу SSH

##### Часть 3. Настройка коммутатора для доступапо протоколу SSH

##### Часть 4. SSH через интерфейс командной строки (CLI) коммутатора

#### Инструкции

##### Часть 1. Настройка основных параметров устройства

##### Шаг 1. Создайте сеть согласно топологии.

##### Шаг 2. Выполните инициализацию и перезагрузку маршрутизатора и коммутатора

##### Шаг 3. Настройте маршрутизатор.

a. Подключитесь к маршрутизатору с помощью консоли и активируйте привелегированный режим EXEC

b. Войдите в режим конфигурации

c. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов

d. Назначьте *class* в качестве зашифрованного пароля привелегированного режима EXEC

e. Назначьте *cisco* в качестве пароля консоли и включите вход в систему по паролю

f. Назначьте *cisco* в качетсве пароля VTY  и включите вход в систему по паролю.

g. Зашифруйте открытые пароли.

h. Создайте баннер, который предупреждает о запрете несанкционного доступа.

i. Настройте и активируйте на маршрутизаторе интерфейс g0/0/1, используя информацию, приведенную в таблице адресации.

j. Сохраните текущую конфигурацию в файл загрузочной конфигурации

a-j. 

```
Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#enable secret class
Router(config)#line con
Router(config)#line console 0
Router(config-line)#login
% Login disabled on line 0, until 'password' is set
Router(config-line)#password cisco
Router(config-line)#exit
Router(config)#line vty 0 15
Router(config-line)#login
% Login disabled on line 2, until 'password' is set
% Login disabled on line 17, until 'password' is set
Router(config-line)#password cisco
Router(config-line)#exit
Router(config)#service password-encryption 
Router(config)#banner motd #Unauthorized Access Strictly Prohibited#
Router(config)#inter g0/0/1 
Router(config-if)#no sh
Router#copy running-config startup-config 
Destination filename [startup-config]? startup-config
Building configuration...
[OK]
```

##### Шаг 4. Настройте компьютер PC-A

a. Настройте для PC-A IP-адрес и маску подсети.

b. Настройте для PC-A шлюз по умолчанию.

a-b. 
```
```

![](http://dl3.joxi.net/drive/2021/09/13/0050/1314/3282210/10/f3d9564215.jpg)

##### Шаг 5. Проверьте подключение к сети.

![](http://dl3.joxi.net/drive/2021/09/13/0050/1314/3282210/10/045bb62764.jpg)

 --------

##### Часть 2. Настройка маршрутизатора для доступа по протоколу SSH

##### Шаг 1. Настройте аутентификацию устройств.

a. Задайте имя устройства.

b. Задайте домен для устройства.

a-b.
```Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hostname R1
R1(config)#ip domain-name cisco.com
```

##### Шаг 2. Создайте ключ шифрования с указанием его длины.

```
R1(config)#crypto key generate rsa
The name for the keys will be: R1.cisco.com
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
```

##### Шаг 3. Создайте имя пользователя в локальной базе учетных записей.

```
R1(config)#username admin password Adm1nP@55
```

##### Шаг 4. Активируйте протокол SSH на линиях VTY

a. Активируйте протоколы Telnet и SSH на входящих линиях VTY с помощью команды *transport input*

b. Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей

a-b

```R1(config)#line vty 0 15
R1(config-line)#transport input telnet 
R1(config-line)#transport input ssh
R1(config-line)#login local
```

##### Шаг 5. Сохраните текущую конфигурацию в файл загрузочной конфигурации.

```
R1#copy running-config startup-config 
Destination filename [startup-config]? startup-config
```


##### Шаг 6. Установите соединение с маршрутизатором по протоколу SSH

a. Запустите TeraTerm с PC-A

b. Установите SSH-подключениек R1. Используйте username **admin** и пароль **Admin1nP@55**

a.

![](http://dl3.joxi.net/drive/2021/09/13/0050/1314/3282210/10/a839a190eb.jpg)

b.

![](http://dl3.joxi.net/drive/2021/09/13/0050/1314/3282210/10/3c6b7bdf72.jpg)

   ----------
  
#### Часть 3. Настройка коммутатора для доступа по протоколу SSH

##### Шаг 1.

a. Подключитесь к коммутатору с помощью консольного подключения и активируйте привелегированный режим EXEC

b. Войдите в режим конфигурации

c. Отключите поиск DNS, чтобы предотвратить попытки маршрутизатора неверно преобразовывать введенные команды таким образом, как будто они являются именами узлов

d. Назначьте *class* в качестве зашифрованного пароля привелегированного режима EXEC

e. Назначьте *cisco* в качестве пароля консоли и включите вход в систему по паролю

f. Назначьте *cisco* в качетсве пароля VTY  и включите вход в систему по паролю.

g. Зашифруйте открытые пароли.

h. Создайте баннер, который предупреждает о запрете несанкционного доступа.

i. Настройте и активируйте на маршрутизаторе интерфейс VLAN 1, используя информацию, приведенную в таблице адресации.

j. Сохраните текущую конфигурацию в файл загрузочной конфигурации

a-j.

```Switch>
Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#enable secret class
Switch(config)#line console 0
Switch(config-line)#login
% Login disabled on line 0, until 'password' is set
Switch(config-line)#password cisco
Switch(config-line)#exit
Switch(config)#line vty 0 15
Switch(config-line)#login
% Login disabled on line 1, until 'password' is set
% Login disabled on line 16, until 'password' is set
Switch(config-line)#password cisco
Switch(config-line)#exit
Switch(config)#service password-encryption 
Switch(config)#inter vlan 1
Switch(config-if)#ip ad 192.168.1.11 255.255.255.0
Switch(config-if)#no sh

Switch(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up

Switch(config-if)#exit
Switch(config)#end
Switch#
%SYS-5-CONFIG_I: Configured from console by console

Switch#copy run startup-config 
Destination filename [startup-config]? startup-config
Building configuration...
[OK]
Switch#

```


##### Шаг 2. Настройте коммутатор для соединения по протоколу SSH.

a. Настройте имя устройства, как указано в таблице адресации.

b. Задайте домен устройства.

c. Создайте ключ шифрования с указанием его длины.

d. Создайте имя пользователя в локальной базе учетных записейю

e. Активируйте протоколы Telnet и  SSH на линиях VTY

f. Измените способ входа в систему таким образом, чтобы использовалась проверка пользователей по локальной базе учетных записей.

a-f.

```
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hostname S1
S1(config)#ip domain-name cisco
S1(config)#ip domain-name cisco.com
S1(config)#crypto key generate rsa
The name for the keys will be: S1.cisco.com
Choose the size of the key modulus in the range of 360 to 2048 for your
  General Purpose Keys. Choosing a key modulus greater than 512 may take
  a few minutes.

How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]

S1(config)#
*Mar 1 1:18:15.919: %SSH-5-ENABLED: SSH 1.99 has been enabled
S1(config)#username admin password Admin@55
S1(config)#line vty 0 15
S1(config-line)#login local	
S1(config-line)#transport input telnet
S1(config-line)#transport input ssh
S1(config-line)#exit
```


##### Шаг 3. Установите соединение с коммутатором по протоколу SSH.

![](http://dl4.joxi.net/drive/2021/09/13/0050/1314/3282210/10/402e8334ac.jpg)

![](http://dl3.joxi.net/drive/2021/09/13/0050/1314/3282210/10/ee3ea2a753.jpg)

 -------
 
#### Часть 4. Настройка протокола SSH с использованием интерфейса командной строки (CLI) коммутатора

##### Шаг 1. Посмотрите доступные параметры для клиента SSH в CISCO IOS

##### Шаг 2. Установите с коммутатора S1 соединение с маршрутизатором R1 по протоколу SSH

a-d.

```
S1#ssh ?
  -l  Log in using this user name
  -v  Specify SSH Protocol Version
S1#ssh -l admin 192.168.1.1

Password: 

Unauthorized Access Strictly Prohibited


R1>
S1#
S1#
[Resuming connection 1 to 192.168.1.1 ... ]

R1>en
Password: 
Password: 
R1#exit

[Connection to 192.168.1.1 closed by foreign host]
S1#
```

Какие версии протокола SSH поддерживаются при использовании интерфейса командной строки?


##### Вопрос для повторения

Как предоставить доступ к сетевому устройству нескольким пользователям, у каждого из которых есть собственное имя

- создать в локальной базе устройства учетную запись, то есть имя пользователя и пароль




