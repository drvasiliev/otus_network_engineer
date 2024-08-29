#### Основные протоколы сети интернет
 1. [VPN. GRE. DmVPN](configs/).

##### Задачи:
1.
2. 
3. 
4. 
5. 
6. 
7. 
8. 
### Задча: 1. 
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