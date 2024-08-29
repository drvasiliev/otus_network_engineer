#### Основные протоколы сети интернет
 1. [VPN. GRE. DmVPN](configs/).

##### Задачи:
1. Настроите GRE между офисами Москва и С.-Петербург.
2. Настроите DMVMN между Москва и Чокурдах, Лабытнанги.
3. Все узлы в офисах в лабораторной работе должны иметь IP связность.

### Задча: 1. Настроите GRE между офисами Москва и С.-Петербург.
- Создадим access-list, полче чего создадим правило nat
 R14
```
# access-list 100 permit ip 10.70.0.0 0.0.255.255 any
# ip nat inside source list 100 interface Ethernet0/2 overload

# int e0/2
    ip nat outside
    ip nat enable

# int loopback 1
    ip nat inside

```

### Задча: 2. Настроите DMVMN между Москва и Чокурдах, Лабытнанги.