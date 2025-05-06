# Часть 1. Создание и настройка сети
## Шаг 1. Подключите сеть в соответствии с топологией.
![](Pics\Lab2-1-1.png)
## Шаг 2. Настройте узлы ПК.
![](Pics\Lab2-1-2.png)
## Шаг 3. Выполните инициализацию и перезагрузку коммутаторов.
Для инициализации коммутатора нам необходимо удалить файл конфигурации виртуальных сетей и startup-config, после чего выполнить перезагрузку.

Проверяем наличие файла vlan.dat:
```
Press RETURN to get started!

 Unauthorized access is strictly prohibited.

User Access Verification

Password: 

S1>en
Password: 
S1#sh fl
S1#sh flash: 
Directory of flash:/

    1  -rw-     4670455          <no date>  2960-lanbasek9-mz.150-2.SE4.bin
    2  -rw-        1318          <no date>  config.text

64016384 bytes total (59344611 bytes free)
```
В моем случае его нет. Если бы был, то удаляем его командой delete vlan.dat

Стираем загрузочный конфиг:
```
S1#erase start
S1#erase startup-config 
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]
[OK]
Erase of nvram: complete
%SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram
```
И перезагружаем коммутатор:
```
S1#reload
Proceed with reload? [confirm]
```
## Шаг 4. Настройте базовые параметры каждого коммутатора.

### a.	Настройте имена устройств в соответствии с топологией.

```
Switch>en
Switch#hostname S1
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#hos
Switch(config)#hostname S1
S1(config)#

```
### b.	Настройте IP-адреса, как указано в таблице адресации.
```
S1>en
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#int vlan1
S1(config-if)#ip add
S1(config-if)#ip address 192.168.1.11 255.255.255.0
S1(config-if)#no sh
S1(config-if)#no shutdown 

S1(config-if)#
%LINK-5-CHANGED: Interface Vlan1, changed state to up

%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
int fa 0/6
S1(config-if)#no sh
S1(config-if)#int fa 0/1
S1(config-if)#no sh
S1(config-if)#exit
```
### c.	Назначьте cisco в качестве паролей консоли и VTY.
```
S1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
S1(config)#line vty 0 15
S1(config-line)#password cisco
S1(config-line)#login
S1(config-line)#
```
### d. 	Назначьте class в качестве пароля доступа к привилегированному режиму EXEC.
```
S1(config)#service password-encryption
S1(config)#enable secret class
S1(config)#end
S1#
```
Аналогично проделываем для второго коммутатора.
Для базовой настройки коммутаторов, с целью сокращения времени в будущем, написал следующую последовательность:
```markdown
en
conf t
no ip domain-lookup
hostname *S2*
banner motd *# Unauthorized access is strictly prohibited.#*
int vlan1
ip address *192.168.1.12 255.255.255.0*
no shutdown
exit
interface *fa 0/18*
no shutdown
interface *fa 0/1*
no shutdown
exit
service password-encryption
enable secret class
line con 0
password *cisco*
login
logging  synchronous 
end
conf t
line vty *0 15*
password *cisco*
login
end

```
Переменные выделены