# Siete - Commands

Prikazy na konfiguraciu v Cisco IOS  
Podrobnejsie poznamky v subore [Siete.md](./Siete.md)

# Basic config

```
hostname
banner motd ""
line console 0
    logg sync
no ip domain lookup

username student secret student
username admin priv 15 secret admin
line con 0
    login local
line vty 0 15
    login local
    transport input ssh
service password-encryption
enable secret class

ip domain-name b303.sk
crypto key generate rsa
    1024
ip ssh version 2
```

# Staticke smerovanie

```
ip route {siet} {maska} {vystupna siet | vystupne rozhranie} [metrika]

ip route 192.168.1.2 255.255.255.0 s0/0/0
ip route 192.168.1.2 255.255.255.0 192.168.99.1
ip route 192.168.1.2 255.255.255.0 g0/0 192.168.10.1

ip route 0.0.0.0 0.0.0.0 192.168.1.1
```

Show Commands

```
do show ip route
do show ip route static
do show ip route 192.168.1.0

do show ip int b

do show cdp neighbors
do show cdp neighbors detail
```

IPv6

```
ipv6 unicast-routing
ipv6 route {siet/prefix} {vystupna siet | vystupne rozhranie} [metrika]

ipv6 route ::/0 s0/0/0
```

# RIP

```
router rip
    version 2
    no auto-summary
    network 192.168.1.0
    network 192.168.2.0

    passive-int g0/0

    passive-int default
    no passive-int g0/0

    redistribute static
    redistribute connected

    default-information originate

ip route 192.168.1.0 255.255.255.0 Null0
```

Show commands

```
do show ip route
do show ip route rip
do show ip protocols
```

Manualna sumarizacia

```
int g0/0
    ip summary-address rip 192.168.1.0 255.255.255.0
```

RIPng

```
ipv6 unicast-routing

int g0/0
    ipv6 rip CUSTOM_NAZOV enable
    ipv6 rip CUSTOM_NAZOV default-information originate

int g0/1
    ipv6 rip CUSTOM_NAZOV enable

do show ipv6 protocols

```

Autentifikacia

```
key chain KLUCENKA
    key 1
        key-string h@slisko

int g0/0
    ip rip auth mode {md5 | text}
    ip rip auth key-chain KLUCENKA

do show key chain
do show ip proto
```

# VLAN

```
delete vlan.dat

vlan 10
    name studenti

int g0/1
    switchport mode access
    switchport access vlan 10

int range g0/2 - 4
    switchport trunk encapsulation dot1q
    switchport mode trunk
    switchport trunk native vlan 100
    switchport trunk allowed vlan 10,20,30
    switchport voice vlan 150
    switchport nonegotiate

vlan 99
    name management
int vlan 99
    ip add 192.168.1.2
```

Show commands

```
do show vlan
do show vlan brief
do show vlan 10

do show interface trunk
do show interface g0/0 switchport
do show dtp int g0/1
```

Zmena `trunk` -> `access`

```
int g0/1
    no switchport trunk allowed vlan
    no switchport trunk native vlan
    switchport mode access
```

DTP (dynamicke urcenie `trunk`/`access`)

```
int g0/1
    switchport mode dynamic desirable  # trunk
    switchport mode dynamic auto  # access
```

VTP (redistribucia VLAN)

```
vtp domain MENO_DOMENY
tp mode {client | server | transparent}
vtp password h@slisko
vtp version {1 | 2}

vtp prunning  # iba servery

do show vtp status
do show vtp counters
```

**Smerovanie medzi VLAN**
Router on Stick

```
int g0/0.10
    encapsulation dot1q 10
    ip address 192.168.10.1 255.255.255.0

int g0/0.20
    encapsulation dot1q 20
    ip address 192.168.20.1 255.255.255.0
```

Multi-layer switching

```
ip routing

vlan 10,20,30
    exit

int vlan 10
    ip add 192.168.10.1 255.255.255.0

int vlan 20
    ip add 192.168.20.1 255.255.255.0
```

Enable QoS

```
mls qos trust cos
```

# STP

```
spanning-tree vlan 1
no spanning-tree vlan 1

spanning-tree vlan 1 priority PRIORITY
spanning-tree vlan 1 root primary
spanning-tree vlan 1 root secondary

int g0/1
    spanning-tree portfast

spanning-tree portfast default
```

Show commands

```
do show spanning-tree
do show spanning-tree summary
do show spanning-tree detail
do show spanning-tree vlan 1
do show spanning-tree active
```

RPVST

```
spanning-tree mode rapid-pvst

spanning-tree vlan 1 priority PRIORITY
spanning-tree vlan 1 root primary
spanning-tree vlan 1 root secondary

int g0/1
    spanning-tree portfast
    spanning-tree link-type point-to-point
```

# ACL

Standard ACL

```
access-list 10 remark Nejaky randomny ACL
access-list 10 deny host 192.168.1.2 [log]
access-list 10 permit 192.168.1.0 0.0.0.255 [log]
access-list 10 deny any [log]

ip access list standard MOJ_PRVY_ACL
    permit host 192.168.1.2
    deny host 192.168.1.3
    permit 192.168.1.0 0.0.0.255

    5 permit host 192.168.1.10
    no 10
```

Extended ACL

```
access-list 110 {pemit | deny} {tcp | udp | ip | ...} {source wildcard_mask} [operator port] {destination wildcard_mask} [operator port]

access-list 110 permit udp 192.168.1.0 0.0.0.255 any eq 69
access-list 110 permit tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 110 permit tcp 192.168.1.0 0.0.0.255 any eq 443

ip access-list extended MOJ_DRUHY_ACL
    permit ip any any
    ...
```

Aplikovanie ACL

```
int g0/0
    ip access-group {name | number} {in | out}
    ip access-group 10 out

line vty 0 15
    access-class 11 in
```

Show commands

```
do show access-list
do show access-list 10
do show ip access-list

do show run | include access-list
do show run | include access-list 10
```

Vymazanie ACL

```
int g0/0
    no ip access-group 10

no access-list 10
```

# EtherChannel & FHRP

HSRP

```
int f0/0.1
 encapsulation dot1q 1
 ip add 192.168.1.101 255.255.255.0
 standby version 2
 standby 1 priority 150
 standby 1 ip 192.168.1.1
 standby 1 preempt

int f0/0.2
 encapsulation dot1q 2
 ip add 192.168.1.101 255.255.255.0
 standby version 2
 standby 2 ip 192.168.2.1
 standby 2 preempt
```

HSRP show commands

```
do show standby
do show standby brief

debug standby packets
debug standby packets terse
```

PAgP

```
int range f0/1 - f0/2
 channel-group GROUP_NUMBER mode MODE
 [channel-port {pagp | lacp}]

int port-channel GROUP_NUMBER
 # dalsia konfiguracia ako normalne
 sw mode access
 sw access vlan 10
 ...

port-channel load-balance TYPE
do show etherchannel load-balance
```

EtherChannel show commands

```
do show ip int b

do show etherchannel
do show etherchannel summary
do show etherchannel port-channel
do show etherchannel GROUP_NUMBER port-channel
do show etherchannel detail
do show etherchannel load-balance

do show interface etherchannel
do show interface TYPE SPEC etherchannel
```

Vymazanie etherchannel

```
no int port-channel 1
int range f0/1 - f0/2
 no channel-group 1 MODE
 no shut
```

# DHCP

DHCP server

```
service dhcp  # on by default

ip dhcp pool NAZOV_POOLU
 network 192.168.1.0 255.255.255.128
 default-router 192.168.1.1
 dns-server 195.146.132.59
 domain-name b303.sk
 lease {days [hours [minutes]] | infinite}
 ?

ip dhcp excluded-address 192.168.1.1 [192.168.1.10]
```

Show commands

```
do show ip dhcp binding
do show ip dhcp server statistics
do show ip dhcp pool
do show ip dhcp conflict

do show run | section dhcp
do show run | include no service dhcp  # ci nie je dhcp vypnute
```

DHCP client (Cisco router) (neodporuca sa)

```
int g0/0
 ip address dhcp

do show ip int g0/0
```

Windows

```
ipconfig /release
ipconfig /renew
ipconfig /all
ipconfig /?
```

Relay Agent

> IP adresa servera za routerom

```
int g0/0
 ip helper-address 192.168.2.1
```

# NAT

Konfiguracia

```
int g0/0
    ip nat inside  # vnutorne, privatne adresy

int g0/1
    ip nat outside  # vonkajsie, verejne adresy
```

Pri pouziti VLAN sa pouzivaju subinterfaces (logicke rozhranie)

```
int g0/0.10
    ip nat inside
int g0/0.20
    ip nat inside
```

Show prikazy

```
do show ip nat translations
do show ip nat statistics

do show run | include nat
do debug ip nat
```

Staticky NAT preklad

```
ip nat inside source static INSIDE_LOCAL_ADD INSIDE_GLOBAL_ADD
ip nat inside source static 192.168.1.20 200.1.1.1
```

Dynamicky NAT preklad

```
ip nat pool MENO_POOLU FIRST_IP LAST_IP netmask MASKA
ip nat pool MOJ_ROZSAH 211.2.2.8 211.2.2.10 netmask 255.255.255.252

access-list CISLO_ACL_LISTU permit SOURCE WILDCARD_MASK
access-list 1 permit 10.1.1.0 0.0.0.255

ip nat inside source list CISLO_ACL_LISTU pool MENO_POOLU
ip nat inside source list 1 pool MOJ_ROZSAH
```

Vymazanie NAT tabulky

```
clear ip nat translations *
```

PAT

```
access-list CISLO_ACL_LISTU permit SOURCE WILDCARD_MASK
ip nat inside source list CISLO_ACL_LISTU interface s0/0 overload

access-list 1 permit 10.1.1.0 0.0.0.255
access-list 1 permit 20.1.1.0 0.0.0.255
ip nat inside source list 1 interface s0/0 overload
```

Pretazenie adresneho rozsahu (`n:m`)

```
access-list CISLO_ACL_LISTU permit SOURCE WILDCARD_MASK
ip nat pool MENO_POOLU FIRST_IP LAST_IP netmask MASKA

ip nat inside source list CISLO_ACL_LISTU pool MENO_POOLU overload
```

Port Forwarding

```
ip nat inside source static tcp 192.168.10.254 80 209.165.200.255 8080
```
