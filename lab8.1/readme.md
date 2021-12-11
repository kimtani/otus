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


Router>en
Router#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Router(config)#hos
Router(config)#hostname R2
R2(config)#no ip domain-name
R2(config)#ena
R2(config)#enable secret class
R2(config)#line con 0
R2(config-line)#cisoc
                ^
% Invalid input detected at '^' marker.
	
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#line vty 0 4
R2(config-line)#pass
R2(config-line)#password cisco
R2(config-line)#login
R2(config-line)#exit
R2(config)#ser
R2(config)#service pas
R2(config)#service password-encryption 
R2(config)#banner motd
% Incomplete command.
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









R2 con0 is now available






Press RETURN to get started.
