# PS2 - Commands

Prikazy PS2

## EIGRP

Show prikazy

```
do show ip eigrp neighbors
do show ip eigrp topology
do show ip eigrp topology all-links  # aj susedia co nesplnaju FC

do show ip eigrp interfaces
do show ip protocols
do show ip eigrp traffic
do show ip eigrp events
```

Konfiguracia

```
router eigrp AUTONOMOUS-SYSTEM-NUMBER
    no auto-summary
    network NUMBER [WILDCAD-MASK]
    network NUMBER [NETWORK-MASK]

    eigrp router-id IPV4-ADDRESS

    passive-interface g0/0
    passive-interface default  # nastavi vsetky

int g0/0
    bandwidth KILOBITS
    delay TENS_OF_MICROSECONDS
```

Ak treba ovplyvnit vyber cesty tak nepouzivat `bandwidth`, pouzivat `delay`

Overenie

```
do show ip eigrp neighbors
do show ip eigrp neighbors detail

do show ip eigrp interfaces
do show ip eigrp interfaces detail

do show ip protocols
do show ip eigrp topology
do show ip eigrp topology all-links

do show ip route eigrp
do show ip eigrp traffic
do show ip eigrp events
```

IPv6

```
ipv6 unicast-routing
int g0/0
    ipv6 eigrp 1

ipv6 router eigrp AUTONOMOUS-SYSTEM-NUMBER
    eigrp router-id 1.1.1.1
    no shut

int g0/1
    ipv6 eigrp 1
```

Redistribucia IPv6 default route

```
ipv route ::/0 OUT_INTERFACE NEXT_HOP_IP  # odporuca sa next hop IP
ipv router eigrp 1
    redistribute static
    redistribute static metric BW DEL REL LOAD MTU
```

Manualna Sumarizacia

```
router eigrp AS
    no auto-summary

int g0/0
    ip summary-address eigrp AS SIET MASKA
    ipv summary-address eigrp AS IPV6_ADDRESS [ADMIN_DISTANCE]
```

Autentifikacia (iba MD5)

```
key chain MENO
    key CISLO
        key-string HESLO
    key INE_CISLO
        key-string INE_HESLO

int g0/0
    ip authentication mode eigrp AS md5
    ip authentication key-chain eigrp AS MENO

do show key chain
```

Zmena casovacov

```
ip hello-interval eigrp AS_NUMBER HELLO_INTERVAL
ip hold-time eigrp AS_NUMBER HOLD_TIME

ipv hello-interval eigrp AS_NUMBER HELLO_INTERVAL
ipv hold-time eigrp AS_NUMBER HOLD_TIME
```

Load balancing

```
router eigrp 100
    variance 2
    maximum-paths 3  # LB nad max 3 cestami
ip hello-interval eigrp AS_NUMBER HELLO_INTERVAL
ip hold-time eigrp AS_NUMBER HOLD_TIME
```

## OSPF

```
do show ip protocols
do show ip route
do show ip route ospf

do show ip osfp neighbor
do show ip osfp neighbor detail
do show ip osfp database
do show ip osfp interface g0/0
do show ip ospf
```

OSPF process, routes, networks + more

```
router ospf PROCESS-ID  # lokalne cislo pre dany router
 network IP-ADD WILDCARD area AREA-ID

 # pri zmene router id treba aj restartovat proces
 router-id 1.1.1.1
 do clear ip ospf process

 area 2 range IP_ADDRESS MASK  # sumarizacia (iba na ABR)

 passive-interface g0/0
 passive-interface default  # vsetky

 default-information originate [always]

 ip ospf hello-interval SECONDS
 ip ospf dead-interval SECONDS

 area 1 auth message-digest

router ospf PROCESS-ID [vfr VPN-NAME]

int g0/0
 ip ospf PROCESS-ID area AREA-ID
 ip ospf PROCESS-ID area AREA-ID [secondaries none]
```

Restart procesu - volba DR/BDR

```
do clear ip ospf process
```

Zmena metriky

```
router ospf PROCESS-ID
 auto-cost reference-bandwidth REF_BANDWIDTH  # prepocita sa (REF_BANDWIDTH/bandwidth)

int g0/0
 ip ospf NEW_COST_VALUE
```

Zmena priority

```
int g0/0
 ip ospf priority NUMBER  # 0-255
```

Autentifikacia

```
# md5
router ospf 1
 area 1 auth message-digest

int g0/0
 ip ospf message-digest-key 1 md5 HESLO

# plaintext
router ospf 1
 area 1 auth

int g0/0
 ip ospf authentication-key HESLO
```

### IPv6

```
ipv router ospf 1
 router-id 1.1.1.1
 passive-interface g0/0

int g0/0
 ipv ospf 1 area 1
```

## HDLC

```
int s0/0/0
    encapsulation hdlc

do show int s0/0/0
do show ip int brief
do show controller s0/0/0  # ci je DCE alebo DTE
```

## PPP

```
int s0/0/0
    encapsulation ppp
    compress ?
    ppp quality ?
    ppp multilink ?

do show ip int brief
```

#### PAP

Klient

```
int s0/0/0
    encapsulation ppp
    ppp pap sent-username MENO password HESLO
```

Server

```
username MENO password HESLO
int s0/0/0
    encapsulation ppp
    ppp authentication pap
```

Ak chceme obojsmerny auth, treba obidve spravit na obidvoch zariadeniach

#### CHAP

Kedze sa predstavuju pomocou hostname, tak ja musim mat v databaze meno toho druheho, obidvaja musime mat rovnake heslo

Klient

```
username MENO-SERVERA password HESLO
int s0/0/0
    encapsulation ppp
```

Server

```
username MENO-KLIENTA password HESLO
int s0/0/0
    encapsulation ppp
    ppp authentication chap
```

Mozeme kombinovat ze aj PAP aj CHAP naraz

## PPPoE

### Rozsierenie konfiguracie PPP a pridelenim IP a default route

R1 - Server - Prideli IPcku

```
username R2 password cisco2
ip local pool PPP_POOL 10.0.0.10 10.0.0.20

int s0/0/0
    ip add 10.0.0.1 255.0.0.0
    encapsulation ppp
    peer default ip address pool PPP_POOL
    ppp authentication pap
```

R2 - Klient - Dosatne IPcku

```
int s0/0/0
    ip address negotiated  ! vypyta si ipcku
    encapsulation ppp
    ppp pap sent-username R2 password cisco2
    ppp ipcp route default
```

### Dialer?

PPPoE server

```
int lo 0
    ip add 10.0.0.254 255.255.255.0

username pouzivatel1 password heslo heslo1
username pouzivatel1 autocommand logout

ip local pool PPPoE-POOL 10.0.0.10 10.0.0.20

interface virtual-template 1
    ip unnumbered loop 0
    mtu 1492
    ppp mtu adaptive
    ip tcp adjust-mss 1452
    peer default ip address pool PPPoE-POOL
    ppp authentication chap
    ppp ipcp dns 8.8.8.8

bba-group pppoe global
    virtual-template 1

int f0/0
    pppeo enable group global
```

PPPoE klient

```
interface dialer 1
    encapsulation ppp
    mtu 1492
    dialer pool 1

    ppp chap hostname pouzivatel1
    ppp chap password heslo1

    ppp ipcp route default
    ! dialer persistent

! ip route 0.0.0.0 0.0.0.0 dialer 1

int f0/0
    no ip address
    pppoe enable group global
    pppoe-client dial-pool-number 1
    no shut
```

PC s WIN ako klient

```
int lo 0
    ip add 10.0.0.1 255.255.255.255

bba-group pppoe global
    virtual template 1
    sesions per-mac throttle 100 1 2

ip local pool ADRESY-PPPoE-KLIENTOV 192.168.1.1 192.168.1.254

interface virtual-template 1
    ip unnumbered loopback 0
    peer default ip address pool ADRESY-PPPoE-KLIENTOV
    mtu 1492
    ppp mtu adaptive
    ip tcp adjust-mss 1452
    ppp authentication ms-chap-v2 ms-chap chap
    ppp ipcp dns 8.8.8.8

int g0/0
    pppoe enable group global
    no shut

username user1 priv 0 password heslo1
username user2 priv 0 password heslo2
username user1 autocommand logout
username user2 autocommand logout
```

### Overenie a diagnostika

```
do show ip int b
do show interface dialer
do show pppoe session
```

## BGP

Zapnutie BGP

```
router bgp AS-NUMBER
    neighbor IP-ADDRESS remote-as AS-NUMBER
    network NETWORK-ADDRESS [mask NETWORK-MASK]
```

Show prikazy

```
do show ip bgp neigh
do show ip bgp summary
do show ip bgp
do show ip route bgp
```

## Sprava zadiadeni

### NTP

```cisco
ntp server IP-ADDRESS
do show ntp associations
do show ntp status
do show clock [detail]
```

### Syslog

```cisco
logging monitor LEVEL
logging IP-ADD
logging trap LEVEL
logging source-interface g0/0

do show logging
```

## Udrzba zariadeni

```cisco
! general
show file systems
dir
pwd
cd

! zaloha na tftp
copy run tftp
copy start tftp

! usb porty
show file systems
dir usbflash0:
copy run usbflash0:/

! sprava obrazov
show flash
copy SOURCE tftp
copy tftp: DESTINATION-URL
boot system FILE-URL

! licencie
show license udi
license install LOCATION
reload
show version
show license
license accept end user agreement
! vela dalsich srandiciek ...

```

## Bezpecnost

### SNMP

```cisco
access-list 1 permit 10.1.1.0 0.0.0.255
snmp-server community cisco RO 0  ! read-only, 0=ACL
snmp-server community xyz123 RW 1  ! read-write, 1=ACL
snmp-server location LOCATION-NAME
snmp-server contact ADMIN-NAME
snmp-server host 10.1.1.50 xyz123
snmp-server enable traps ?

do show snmp
do show snmp community
```

### SPAN

```cisco
monitor session NUMBER source [ int g0/0 | vlan 1 ]
monitor session NUMBER destination [ int g0/0 | vlan 1 ]
```
