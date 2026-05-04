### **🔴 SEMESTER 2**

##### Static Routing

adalah metode routing dengan menambahkan satu persatu route oleh administrator

```bash

ena
conf
int <interfaces>
no shut
ip add <ip address> <subnet>
ip route <network destination> <subnet> <gateway>

```

Wild card bits = subnet mask(/32) - current subnet
**IP Routing Through Public IP**

```bash
   ena
   conf
   int g0/0
   no shut
   ip add <ip address>
   ip route 0.0.0.0 0.0.0.0 202.200.1
   int g0/0
   ip nat outside
   int g1/0
   ip nat inside
   access-list 1 permit <Ip Private> <wild card bits>
   ip nat inside source list 1 interface g0/0 overload
```

![alt text](<IaaS 20_04_2026-1.PNG>)

##### router-NAT

```bash
    ena
    conf t

    interface g0/1.10
    encapsulation dot1Q 10
    ip address 192.168.10.126 255.255.255.128
    ip nat inside

    interface g0/1.20
    encapsulation dot1Q 20
    ip address 192.168.10.254 255.255.255.128
    ip nat inside

    access-list 1 permit 192.168.10.0 0.0.0.127
    access-list 1 permit 192.168.10.128 0.0.0.127
    ip nat inside source list 1 interface g0/0 overload
    ip route 0.0.0.0 0.0.0.0 202.20.20.254

    int g0/0
    ip add 202.20.20.1 255.255.255.0
    ip nat outside
    no shut
    ------
    int g0/1
    no ip add
    no shut
    ------
```

##### SwitchVLAN

```bash
    vlan 10
    name Guru

    vlan 20
    name Siswa

    int f0/1
    switchport mode access
    switchport access vlan 10

    int f0/2
    switchport mode access
    switchport access vlan 20

    int g0/1
    switchport mode trunk
```

##### PC0

```bash
    IP Address: 192.168.10.1
    Subnet Mask: 255.255.255.128
    Default Gateway: 192.168.10.126
    DNS Server: 100.10.10.3
```

#### ROUTER2

```bash
    ena
    conf t
    int g0/0
    ip add 202.20.20.254
    no shut

    ip route 192.168.10.0 255.255.255.0 202.20.20.1
```

![alt text](image-1.png)

#### R-left

```bash
    ena
    conf
    int g1/0
    ip add 192.168.10.1 255.255.255.0
    no shut
    int g0/0
    ip add 1.1.1.1 255.255.255.252
    no shut
    ----
    ip route 192.168.20.0 1.1.1.2
```

#### Border

```bash
    ena
    conf
    int g0/0
    ip add 1.1.1.2
    no shut
    int g1/0
    ip add 1.1.1.5
    no shut
    ----
    ip route 192.168.10.0 255.255.255.0 1.1.1.1
    ----
    router rip
    version 2
    no auto-summary
    network 1.0.0.0
    redistribute static metric 2
    end
```

#### R-right

```bash
    ena
    conf
    int g0/0
    ip add 1.1.1.6
    no shut
    int g1/0
    ip add 192.168.20.1 255.255.255.0
    no shut
    ----
    router rip
    version 2
    no auto-summary
    network 1.0.0.0
    network 192.168.20.0
    end
```
