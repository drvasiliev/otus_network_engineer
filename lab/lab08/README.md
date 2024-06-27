#### EIGRP

##### Задачи:
1.В офисе С.-Петербург настроить EIGRP.
2.R32 получает только маршрут по умолчанию.
3.R16-17 анонсируют только суммарные префиксы.
4.Использовать EIGRP named-mode для настройки сети.
Настройка осуществляется одновременно для IPv4 и IPv6.
Готовые конфигурации необходимо оформить на github с описанием проделанной работы, используя markdown.

###    ЧАСТЬ 1. Настройка EIGRP

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
