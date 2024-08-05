#### OSPF
1. [Таблица ip aдресов](files/).
2. [Схема сети с ip адресами](../../lab/Otus_lab.drawio).
3. [Конфигурации роутеров](configs/).


#### Задачи:
```
1.Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.
2.Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.
3.Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.
4.Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.
```
###### Маршрутизаторы R14-R15 находятся в зоне 0 - backbone.
```
R14
router ospf 1
 router-id 14.14.14.14
 network 10.70.14.0 0.0.0.7 area 0
 network 10.70.14.100 0.0.0.3 area 0
 network 10.70.14.128 0.0.0.3 area 0
 network 10.70.19.100 0.0.0.3 area 0


```

```
R15
router ospf 1
 router-id 15.15.15.15
 network 10.70.15.0 0.0.0.7 area 0
 network 10.70.15.100 0.0.0.3 area 0
 network 10.70.15.128 0.0.0.3 area 0
 network 10.70.20.100 0.0.0.3 area 0

```
###### Маршрутизаторы R12-R13 находятся в зоне 10. Дополнительно к маршрутам должны получать маршрут по умолчанию.

```
R12
router ospf 1
 router-id 12.12.12.12
 network 10.70.12.0 0.0.0.7 area 10
 network 10.70.14.128 0.0.0.3 area 0
 network 10.70.15.100 0.0.0.3 area 0
```
![alt text](image-2.png)


```
R13
router ospf 1
 router-id 13.13.13.13
 network 10.70.13.0 0.0.0.7 area 10
 network 10.70.14.100 0.0.0.3 area 0
 network 10.70.15.128 0.0.0.3 area 0
```
![alt text](image-1.png)
##### Добавление маршрутосв по умолчанию
```
R15
router ospf 1
default-information originate

R14
router ospf 1
default-information originate
```
###### Маршрутизатор R19 находится в зоне 101 и получает только маршрут по умолчанию.
```
Создадим prefix-list для фильтрации всех сетей на in и разрешим только маршрут по умолчанию
После чего применим его на ospf 1
```
до применения фильтра
![alt text](image-6.png)
```
R19

ip prefix-list R19_IN seq 5 deny 10.0.0.0/8 le 32
ip prefix-list R19_IN seq 10 permit 0.0.0.0/0


router ospf 1
 router-id 19.19.19.19
 network 10.70.19.0 0.0.0.7 area 101
 network 10.70.19.100 0.0.0.3 area 0
 distribute-list prefix R19_IN in
```
![alt text](image-3.png)

###### Маршрутизатор R20 находится в зоне 102 и получает все маршруты, кроме маршрутов до сетей зоны 101.
```
Создадим prefix-list для фильтрации  сетей на in и отсечем сеть 10.70.19.0/29
После чего применим его на ospf 1
```
до применения фильтра
![alt text](image-5.png)
```
R20
ip prefix-list test seq 5 deny 10.70.19.0/29 le 32
ip prefix-list test seq 10 permit 10.70.0.0/16 le 32
ip prefix-list test seq 15 permit 0.0.0.0/0


router ospf 1
 router-id 20.20.20.20
 network 10.70.19.0 0.0.0.7 area 0
 network 10.70.20.0 0.0.0.7 area 102
 network 10.70.20.100 0.0.0.3 area 0
 distribute-list prefix test in

```
![alt text](image-4.png)