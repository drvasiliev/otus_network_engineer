#### PBR
1. [Конфигурации роутеров](configs/).
2. [Схема сети с ip адресами](../../lab/Otus_lab.drawio).

##### Задачи:
```
1.Настроите политику маршрутизации для сетей офиса.
2.Распределите трафик между двумя линками с провайдером.
3.Настроите отслеживание линка через технологию IP SLA.(только для IPv4)
4.Настройте для офиса Лабытнанги маршрут по-умолчанию.
План работы и изменения зафиксированы в документации .
```
![alt text](image.png)
```
Так как к задаче я вернулся после натройки isis я сделал редистрбьюцию на R25 и R26 получает информацию о сети 10.30.0.0/16 (Лабытнаги) - эта сеть будет служить в дальнейшем для тестирования настроек в Чокурдах.
```
#### Выполнение работ:

#### 1.Настроите политику маршрутизации для сетей офиса.
```
R28
interface Ethernet0/0
 description to_R26_e0/1
 ip address 10.50.40.1 255.255.255.252
 shutdown
!
interface Ethernet0/1
 description to_R25_e0/3
 ip address 10.40.50.2 255.255.255.252
!
interface Ethernet0/2
 description to_SW29_e0/2
 no ip address
!
interface Ethernet0/2.28
 description SW_29
 encapsulation dot1Q 28
 ip address 10.50.28.1 255.255.255.0
!
interface Ethernet0/2.30
 description VLAN_VPC30
 encapsulation dot1Q 30
 ip address 10.50.30.1 255.255.255.0
!
interface Ethernet0/2.31
 description VLAN_VPC31
 encapsulation dot1Q 31
 ip address 10.50.31.1 255.255.255.0

```
На пк настроена статика, каждый в своем Vlan
```
SW29
interface Ethernet0/0
 description to_VPC30
 switchport access vlan 30
 switchport mode access
!
interface Ethernet0/1
 description to_VPC31
 switchport access vlan 31
 switchport mode access
!
interface Ethernet0/2
 description to_R28_e0/2
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/3
!
interface Vlan28
 ip address 10.50.28.29 255.255.255.0
!
ip forward-protocol nd
!
no ip http server
no ip http secure-server
!
ip route 0.0.0.0 0.0.0.0 10.50.28.1

```
#### 2.Распределите трафик между двумя линками с провайдером.
2.Настраиваем маршурты по умолчанию на R28 
```
conf ter
ip route 0.0.0.0 0.0.0.0 10.40.50.1 - маршрут до R25 основной
ip route 0.0.0.0 0.0.0.0 10.50.40.2 10 - маршурт до R26 запасной


```

```


```


```


```