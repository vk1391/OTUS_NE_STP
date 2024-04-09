# Настройка STP
 ![alt-dtp](https://github.com/vk1391/OTUS_NE_STP/blob/main/stp.jpg "Схема к домашнему заданию №2")
## Часть 1
1. Базовая настройка коммутаторов:
```
hostname x # где х - название коммутатора 
no ip domain-lookup 
enable secret class
line console 0
password cisco
logging synchronous
exit
line vty 0 4
password cisco
login
exit 
service password-encryption 
banner motd $ Authorized Users Only! $
```

2. на коммутаторах настроены ip адреса согласно следующей таблицы:

**Устройство** | **Интерфейс** | **ip address** | **Маска подсети**
 --- | --- | --- | --- 
| S1 | Vlan1 | 192.168.1.1 | 255.255.255.0 
| S2 | Vlan1 | 192.168.1.2 | 255.255.255.0
| S3 | Vlan1 | 192.168.1.3 | 255.255.255.0 

3. Коммутатор S1 пингует коммутаторы S2 и S3, коммутатор S2 пингует коммутатор S3

## Часть 2
1. отключаем все порты на высех коммутаторах
```
S1(config)#int range 
S1(config)#int range e0/0-3
S1(config-if-range)#sh
S1(config-if-range)#shutdown
```
2. делаем транковыми порты e0/1 и e0/3 и включаем их
```
S1(config)#int range e0/1,e0/3
S1(config-if-range)#sw tr encapsulation dot1q \
S1(config-if-range)#sw m tr
S1(config-if-range)#no shutdown
```
3. В результате протокол Spanning tree определил сдедующее
 ![alt-dtp](https://github.com/vk1391/OTUS_NE_STP/blob/main/stp2.jpg "Схема к домашнему заданию №2.2")

- root bridge является коммутатор S1 так как по дефолту приоритеты на коммутаторах одинаковы, а у коммутатора S1 наименьший MAC адрес
- на коммутаторе S3 порт e0/1 в роле Altr(Blocked) так как порт у порта e0/3 BID отправителя меньше(32769.aabb.cc00.1000) , чем у порта e0/1(32769.aabb.cc00.2000).
## Часть 3
 - Изменив стоимость порта прибываещего в состоянии root на коммутаторе S3, другой порт перешёл из состояния altr в состояние desg, так как был изменён Root path cost.Теперь Коммутатор S2 и S3 знают что путь через S3 к root bridge предпочтительней.
## Часть 4
 ![alt-dtp](https://github.com/vk1391/OTUS_NE_STP/blob/main/stp3.jpg "Схема к домашнему заданию №2.3")
