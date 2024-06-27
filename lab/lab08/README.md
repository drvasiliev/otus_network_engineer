#### EIGRP
 1. [Конфигурации устройств](configs/).
##### Задачи:
1.В офисе С.-Петербург настроить EIGRP.
2.R32 получает только маршрут по умолчанию.
3.R16-17 анонсируют только суммарные префиксы.
4.Использовать EIGRP named-mode для настройки сети.
Настройка осуществляется одновременно для IPv4 и IPv6.
Готовые конфигурации необходимо оформить на github с описанием проделанной работы, используя markdown.

###    Задча: 1. Настройка EIGRP

Настройка EIGRP на примере R18
```
- Обновляем eigrp:

Conf ter
Router eigrp 1
	eigrp upgrade-cli NG

После надо добавить сети в eigrp и назначить id:

Router eigrp NG
address-family ipv4 autonomous-system 1
eigrp rouer-id 10.0.0.18
network 10.60.18.0 0.0.0.7
network 10.60.18.100 0.0.0.3
network 10.60.18.128 0.0.0.3

Настройка аутентификации: 
Создаем сам ключ:

Conf ter
key chain kEIGRP – где kEIGRP название 
	key 1 – номер ключа
		key-string cisco – где cisco сам ключ
После чего добавляем на интерфейс аутентификацию 
router eigrp NG   af-interface Ethernet0/1
   authentication mode md5
   authentication key-chain kEIGRP

Выполнить надо с обеих сторон.
```
###    Задча: 2. R32 получает только маршрут по умолчанию.
первый варина создадим prefix-list на R16 в сторону R32 и будем передавать только маршрут по умолчанию 0.0.0.0/0, после чего добавим на интерфейс

```
conf ter
ip prefix-list Q permit 0.0.0.0/0
router eigrp NG
        address-family ipv4 autonomous-system 1
        topology base
        distribute-list Q out e0/3

```
Второй вариант сделать на R16 суммаризацию 0.0.0.0/0 на интерфейсе в сторону R32
```
conf ter
    ip prefix-list Route-R3 permit 0.0.0.0/0
    router eigrp NG
    f-interface e0/3
    summary-address 0.0.0.0 0.0.0.0
```
###    Задча: 3. R16-17 анонсируют только суммарные префиксы.
Настройка суммарного маршрута на R17 в сторону R18
настройка на R16 идентична.
```
conf ter
 router eigrp NG
        #address-family ipv4 autonomous-system 1
        af-interface e0/1
            summary-address 10.60.17.0 255.255.255.0
        
```