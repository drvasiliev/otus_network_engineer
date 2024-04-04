##### Lab - Configure Router-on-a-Stick Inter-VLAN Routing 

##### Задача:
    1.Воссоздать схему сети в eve.ng
    2.Настройте основные параметры маршрутизатора
### Пример настройки:
```
        en
        conf ter
        hostname R1
        no ip domain-lookup
        enable secret class
        line 0
        password cisco
        login
        line vty 0 15
        password cisco
        login
        service passowrd-encryption
        banner motd "unauthorized access is prohibited"
        clock set 17:15:00 4 apr 2024
        copy running-config startup-config
```

3.Настройте основные параметры для каждого коммутатора.
### Пример настройки:
    Настраиватеся аналогично маршрутизатору


 ### Схема сети и ссылка на конечный конфиг устройств:
   
1. [Схема сети](lab01-vlan.png).   

2. [Конфиги устройств](configs/).


###  Графическая схема:

![](lab01-vlan.png)
