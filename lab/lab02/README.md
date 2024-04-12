### Лабораторная работа. Развертывание коммутируемой сети с резервными каналами 

#### Топология
![](lab02-stp.png)

#### Таблица адресации:

    |-------------|------------|-------------|---------------|
    | Устройство  | Интерфейс  | IP-адрес    | Маска подсети |
    |-------------|------------|-------------|---------------|
    |  S1         |  VLAN 1    | 192.168.1.1 |255.255.255.0  |
    |-------------|------------|-------------|---------------|
    |  S2         |  VLAN 1    | 192.168.1.2 |255.255.255.0  |
    |-------------|------------|-------------|---------------|
    |  S3         |  VLAN 1    | 192.168.1.3 |255.255.255.0  |
    |-------------|------------|-------------|---------------|


##### Цели:
    Часть 1. Создание сети и настройка основных параметров устройства
    Часть 2. Выбор корневого моста
    Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов
    Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов


 ###    ЧАСТЬ 1. Создание сети и настройка основных параметров
 ##### Лабораторная работа проводилась в Cisco Pacet Trace
    Пример настройки одного из коммутаторов:

```
en
    conf ter
    hostname S1
    no ip domain-lookup
    enable secret class
line 0
    password cisco
    logging synchronous
    login
line vty 0 4
    password cisco
    login
line vty 5 15
    password cisco
    login

banner motd "unauthorized access is prohibited"

interface vlan 1
    ip address 192.168.1.1 255.255.255.0
    no shutdown

copy running-config startup-config

```

###    ЧАСТЬ 2. Определение корневого моста

#### шаг 1: Отключите все порты на коммутаторах.
Пример настройки на примере коммутатора S1
```
S1(config)#interface range fastEthernet 0/1-24
S1(config-if-range)#shutdown
```

#### Шаг 2:	Настройте подключенные порты в качестве транковых.

```
S1(config)#interface range fastEthernet 0/1-4
S1(config-if-range)#switchport mode trunk
```
#### Шаг 3:	Включите порты F0/2 и F0/4 на всех коммутаторах.

```
S1(config-if)#interface fa0/2
S1(config-if)#no shutdown 
S1(config-if)#interface fa0/4
S1(config-if)#no shutdown 
```
#### Шаг 4:	Отобразите данные протокола spanning-tree.


```
S1#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0005.5E86.6E41
             This bridge is the root
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     0005.5E86.6E41
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/2            Desg FWD 19        128.2    P2p
Fa0/4            Desg FWD 19        128.4    P2p
```

```
S2#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0005.5E86.6E41
             Cost        19
             Port        2(FastEthernet0/2)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     000C.85B3.4D89
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/4            Desg FWD 19        128.4    P2p
Fa0/2            Root FWD 19        128.2    P2p
```

```
S3#show sp
S3#show spanning-tree 
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0005.5E86.6E41
             Cost        19
             Port        4(FastEthernet0/4)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00E0.F731.409B
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Fa0/4            Root FWD 19        128.4    P2p
Fa0/2            Altn BLK 19        128.2    P2p
```


С учетом выходных данных, поступающих с коммутаторов, ответьте на следующие вопросы:

    1.Какой коммутатор является корневым мостом?  
######    S1
    2.Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?
###### Он был выбран так как у него наименьшее значение MAC адреса
    3.Какие порты на коммутаторе являются корневыми портами? 
###### S2 Fa0/2, S3 Fa0/4.
    4.Какие порты на коммутаторе являются назначенными портами? 
######  S1 Fa0/1-2, S2 Fa0/4.
    5.Какой порт отображается в качестве альтернативного и в настоящее время заблокирован?
###### S3 Fa0/2
    6.Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта?
###### STP блокирует порт Fa0/2 на коммутаторе S3 так как у него самый высокий BID

