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

