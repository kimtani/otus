Лабораторная работа. Развертывание коммутируемой сети в резервными каналами

![](http://joxi.ru/v29zxn8HRyq35A.jpg)


Таблица адресации:

 | Устройство | Интерфейс | IP- адрес | Маска подсети | 
 | ------------- |:------------------:| -----:|-----:|
 | S1 | VLAN 1 | 192.168.1.1 | 255.255.255.0 | 
 | S2 | VLAN 1 | 192.168.1.2 |255.255.255.0|
 | S3 | VLAN 1 | 192.168.1.3 |255.255.255.0|


Шаг 3: Настройте базовые параметры каждого коммутатора.
a.Отключите поиск DNS.

b.Присвойте имена устройствам в соответствии с топологией.

c.Назначьте class в качестве зашифрованного пароля доступа к привилегированному режиму.

d.Назначьте cisco в качестве паролей консоли и VTY и активируйте вход для консоли и VTY
каналов.

e.Настройте logging synchronous для консольного канала.

f.Настройте баннерное сообщение дня (MOTD) для предупреждения пользователей о запрете
несанкционированного доступа.

g.Задайте IP-адрес, указанный в таблице адресации для VLAN 1 на всех коммутаторах.

h.Скопируйте текущую конфигурацию в файл загрузочной конфигурации.

a-h Для S1. S2 и S3 настроены аналогично 


```

Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#line con 0
Switch(config-line)#logg sync
Switch(config-line)#exec-timeout 0 0
Switch(config-line)#exit
Switch(config)#exit
Switch#
%SYS-5-CONFIG_I: Configured from console by console

Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#no ip domain-name
Switch(config)#hostname S1
S1(config)#enable secret class
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#exit
S1(config)#banner motd *Unauthorized access is strictly prohibited*
S1(config)#int vlan 1
S1(config-if)#ip ad 192.168.1.1 255.255.255.0
S1(config-if)#exit
S1(config)#do copy run sta
Destination filename [startup-config]? 
Building configuration...
[OK]
S1(config)#

```

Шаг 4: Проверьте связь.

Проверьте способность компьютеров обмениваться эхо-запросами.

Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?

Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?

Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?

Выполняйте отладку до тех пор, пока ответы на все вопросы не будут положительными.

Все запросы между коммутаторами выполнены успешно.

![](http://joxi.ru/DmB5Q0jUgMYXgr.jpg)
![](http://joxi.ru/L21qKdOUzbxXB2.jpg)
![](http://joxi.ru/v29zxn8HRowE0A.jpg)

-----------------------

### Часть 2: Определение корневого моста

#### Шаг 1: Отключите все порты на коммутаторах.

#### Шаг 2: Настройте подключенные порты в качестве транковых.

#### Шаг 3: Включите порты F0/2 и F0/4 на всех коммутаторах.

#### Шаг 4: Отобразите данные протокола spanning-tree.

![](http://joxi.ru/EA4GRZxCvQ6Q02.jpg)

S1

![](http://joxi.ru/BA0JLWltvnbW8m.jpg)

S2

![](http://joxi.ru/L21qKdOUzbjYW2.jpg)

S3

![](http://joxi.ru/1A503akTzXRV12.jpg)



```
```
```
```
```
```
```
```
```
```
```
```
```
```
```
```


![]()
![]()
![]()
![]()
![]()
![]()
![]()
![]()

