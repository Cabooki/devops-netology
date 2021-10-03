# Домашнее задание к занятию "3.8. Компьютерные сети, лекция 3"

1. Подключитесь к публичному маршрутизатору в интернет. Найдите маршрут к вашему публичному IP
```
telnet route-views.routeviews.org
Username: rviews
show ip route x.x.x.x/32
show bgp x.x.x.x/32
```

```
route-views>show ip route 94.41.85.255
Routing entry for 94.41.84.0/23
  Known via "bgp 6447", distance 20, metric 0
  Tag 6939, type external
  Last update from 64.71.137.241 6w1d ago
  Routing Descriptor Blocks:
  * 64.71.137.241, from 64.71.137.241, 6w1d ago
      Route metric is 0, traffic share count is 1
      AS Hops 2
      Route tag 6939
      MPLS label: none
route-views>show bgp 94.41.85.255
BGP routing table entry for 94.41.84.0/23, version 971109021
Paths: (5 available, best #5, table default)
  Not advertised to any peer
  Refresh Epoch 1
  3267 24955
    194.85.40.15 from 194.85.40.15 (185.141.126.1)
      Origin IGP, metric 0, localpref 100, valid, external
      path 7FE1671D4398 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  20130 6939 24955
    140.192.8.16 from 140.192.8.16 (140.192.8.16)
      Origin IGP, localpref 100, valid, external
      path 7FE03AC2DC30 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 2
  3303 24955
    217.192.89.50 from 217.192.89.50 (138.187.128.158)
      Origin IGP, localpref 100, valid, external
      Community: 3303:1004 3303:1006 3303:1030 3303:1031 3303:3081 24955:310 24955:321 24955:3210 24955:3213 24955:3216 24955:40103 31210:24955 65005:10643
      path 7FE18BE8E888 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  1351 6939 24955
    132.198.255.253 from 132.198.255.253 (132.198.255.253)
      Origin IGP, localpref 100, valid, external
      path 7FE11E6E9E70 RPKI State valid
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  6939 24955
    64.71.137.241 from 64.71.137.241 (216.218.252.164)
      Origin IGP, localpref 100, valid, external, best
      path 7FE18B6F68A8 RPKI State valid
      rx pathid: 0, tx pathid: 0x0
```
****
2. Создайте dummy0 интерфейс в Ubuntu. Добавьте несколько статических маршрутов. Проверьте таблицу маршрутизации.

```
root@vagrant:~# echo "dummy" >> /etc/modules
root@vagrant:~# echo "options dummy numdummies=2" > /etc/modprobe.d/dummy.conf
root@vagrant:~# ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 08:00:27:e3:90:c5 brd ff:ff:ff:ff:ff:ff
3: dummy0: <BROADCAST,NOARP> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether da:71:fe:41:97:05 brd ff:ff:ff:ff:ff:ff
root@vagrant:~# ip addr add 192.168.1.150/24 dev dummy0
root@vagrant:~# ip route add 192.168.1.150 via 10.0.2.2
root@vagrant:~# ip route
default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15
10.0.2.2 dev eth0 proto dhcp scope link src 10.0.2.15 metric 100
192.168.1.150 via 10.0.2.2 dev eth0
```
****
3. Проверьте открытые TCP порты в Ubuntu, какие протоколы и приложения используют эти порты? Приведите несколько примеров.

```
root@vagrant:~# netstat -ltpn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      1/init
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      615/systemd-resolve
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1592/sshd: /usr/sbi
tcp6       0      0 :::111                  :::*                    LISTEN      1/init
tcp6       0      0 :::22                   :::*                    LISTEN      1592/sshd: /usr/sbi
```
`systemd-resolve, sshd` **используют TCP порты.**
****
4. Проверьте используемые UDP сокеты в Ubuntu, какие протоколы и приложения используют эти порты?

```
root@vagrant:~# netstat -lupn
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
udp        0      0 127.0.0.53:53           0.0.0.0:*                           615/systemd-resolve
udp        0      0 10.0.2.15:68            0.0.0.0:*                           410/systemd-network
udp        0      0 0.0.0.0:111             0.0.0.0:*                           1/init
udp6       0      0 :::111                  :::*                                1/init
```
`systemd-resolve, systemd-network` **используют UDP порты.**
****
5. Используя diagrams.net, создайте L3 диаграмму вашей домашней сети или любой другой сети, с которой вы работали. 

**Не уверен, что правильно понял и сделал...**

![1](https://raw.githubusercontent.com/Cabooki/devops-netology/main/03-sysadmin-08-net/scheme.png)