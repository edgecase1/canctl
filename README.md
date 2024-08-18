# canctl


# install
```
apt install can-utils can-gw
```

# test
```
$ sudo docker run -d --rm -ti --name alpine_container alpine 
e48c4157ef42c46f9666da6757298d82ec5b8963ada2137de32cbd7aa3ac0f29

$ sudo ./canctl addifc docker:alpine_container can0 
$ sudo docker exec alpine_container ip link
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: can0: <NOARP,UP,LOWER_UP> mtu 72 qdisc noqueue state UNKNOWN qlen 1000
    link/[280] 
21: eth0@if22: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
```

```
$ sudo ./canctl addbus vcanbr0
4: vcanbr0: <NOARP,UP,LOWER_UP> mtu 72 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/can 

$ sudo ./canctl attachif docker:alpine_container can1 vcanbr0

$ sudo docker exec alpine_container ip link                  
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: can0: <NOARP,UP,LOWER_UP> mtu 72 qdisc noqueue state UNKNOWN qlen 1000
    link/[280] 
21: eth0@if22: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
25: can1@if26: <NOARP,UP,LOWER_UP,M-DOWN> mtu 72 qdisc noqueue state UP qlen 1000
    link/[280] 

$ sudo docker exec alpine_container cangen can1

$ candump vcanbr0                                                              
  vcanbr0  777   [4]  35 46 24 3E
  vcanbr0  09B   [4]  2B C0 3E 7C
  vcanbr0  4B7   [1]  37
  vcanbr0  10A   [8]  83 7C 2D 0E BE 6A 38 0B
  vcanbr0  095   [3]  3B E7 59
```


