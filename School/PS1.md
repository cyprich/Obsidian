# PS1

Poznamocky k predmetu Pocitacove Siete 1

# Basic config

Defaultna konfiguracia ktora by mala byt vykonana vzdy pri novom zariadeni

```
hostname
banner motd
line console 0
 logging sync
no ip domain lookup
```

```
username student secret student
username admin privilege 15 secret admin
line console 0
 login local
line vty 0 15
 login local
service password-encryption
enable secret class
```

```
ip domain-name B303.sk
crypto key gen rsa modulus 1024
ip ssh version 2
line vty 0 15
 transport input ssh
```

# Cisla portov

| Port | Sluzba               | TCP alebo UPD? |
| ---- | -------------------- | -------------- |
| 20   | FTP (data)           | TCP            |
| 21   | FTP (control)        | TCP            |
| 22   | SSH                  | TCP            |
| 23   | Telnet               | TCP            |
| 53   | DNS                  | TCP & UDP      |
| 67   | DHCP (client)        | UDP            |
| 68   | DHCP (server)        | UDP            |
| 69   | TFTP                 | TCP            |
| 80   | HTTP                 | TCP            |
| 443  | HTTPS                | TCP            |
| 520  | RIP                  | UDP            |
| 546  | DHCPv6 (client)      | UDP            |
| 547  | DHCPv6 (server)      | UDP            |
| 1812 | RADIUS auth          | UDP            |
| 1813 | RADIUS accounting    | UDP            |
| 1985 | HSRPv1               | UDP            |
| 5246 | CAPWAP (source)      | UDP            |
| 5247 | CAPWAP (destination) | UDP            |

# Troubleshooting commands

Prikazy ktore len nieco vypisuju

```
do show run
do show ip int b
do show protocols
do show ip route
```

# Staticke smerovanie

```
ip route {siet} {maska} {vystupna siet | vystupne rozhranie} [metrika]
```

Toto basically hovori, ze _"ak pride paket smerujuci do {siet} s maskou {maska}, posli ho na {vystupnu siet/rozhranie}"_

Napriklad `ip route 192.168.1.2 255.255.255.0 s0/0/0` hovori _"ak pride paket, ktory ma ist na adresu 192.168.1.2, posli ho na s0/0/0"_  
Napriklad `ip route 192.168.1.2 255.255.255.0 192.168.99.1` hovori _"ak pride paket, ktory ma ist na adresu 192.168.1.2, posli ho na zariadenie s IP 192.168.99.1"_

Ak je v statickej adrese zadane **vystupne rozhranie**, musime sa uistit ze "za tym rozhranim nieco je" a ze to je prave jeden smerovac ktory bude vediet narabat s paketom  
Ak je v statickej adrese zadana **vystupna IP adresa**, vystupne rozhranie (priamo pripojene) sa zisti rekurzivnym vyhladavanim v smerovacej tabulke

Da sa uviest aj vystupne rozhranie aj adresa - **fully specified**

```
ip route 172.16.1.0 255.255.255.0 g0/0 172.16.2.2 [metrika]
```

## Default route

Normalna staticka cesta, ktora bude vybrata iba ked sa nenajde ziadne ine zhodne rozhranie (longest prefix match)

```
ip route 0.0.0.0 0.0.0.0 192.168.1.1
```

## Floating static routes

_"Pojmom \"floating static route\" sa oznacuje staticky zaznam o nejakej sieti, ktory sa do smerovacej tabulky dostane az vtedy, ked iny zaznam o tej istej sieti zo smerovacej tabulku vypadne"_  
Basically zalozna cesta - ak jeden smerovac/cesta vypadne, bude nahradeny/a inym

Na vytvorenie zaloznej cesty sa zvysi **administrative distance** pri jej vytvarani

```
ip route 0.0.0.0 0.0.0.0 172.16.2.2
ip route 0.0.0.0 0.0.0.0 10.10.10.2 5
```

Prvy zaznam je primarny (metrika = default = 0)  
Druhy zaznam je sekundarny (metrika = 5)

Smerovac vybera zaznam s mensou metrikou

## Sumarne smerovacie zaznamy

_"Sumarizacia je proces, pri ktorom sa isty pocet cielovych sieti popise jednou vacsou sietou"_  
Znizuje sa pocet zaznamov v tabulke

Napr. namiesto sieti...

```
10.1.0.0 /16
10.2.0.0 /16
10.3.0.0 /16
10.4.0.0 /16
```

...mozeme zapisat jeden sumarny zapis ako

```
10.0.0.0 /14
```

## Troubleshooting

Prikazy na overovanie

```
do show ip route
do show ip route static
do show ip route 192.168.1.0

do show ip int b

do show cdp neighbors
do show cdp neighbors detail
```

## IPv6

Basically to iste ako IPv4

```
ipv6 unicast-routing
ipv6 route {siet/prefix} {dalsi smerovac} [metrika]
ipv6 route {siet/prefix} {vystupne rozhranie} [metrika]
ipv6 route {siet/prefix} {dalsi smerovac} {vystupne rozhranie} [metrika]

ipv6 route ::/0 {siet | interface}
```

Sumarny zapis

```
ipv route 2001:db8:acad::/61
```

# RIP

Routing Information Protocol  
RIP je distance-vector dynamicky smerovaci protokol  
Ako metriku pouziva hop  
Adminstrativna vzdialenost je 120

|                   | RIPv1     | RIPv2                   |
| ----------------- | --------- | ----------------------- |
| Updaty cez        | Broadcast | Multicast (`224.0.0.9`) |
| VLSM?             | Nie       | Ano                     |
| CIDR? (classless) | Nie       | Ano                     |
| Sumarizacia       | Nie       | Ano                     |
| Autentifikacia    | Nie       | Ano                     |

Updaty sa posielaju kazdych 30 sekund cez UDP port 520

## Basic config

```
router rip
 version 2
 network 192.168.1.0
 network 192.168.2.0
 ...
 no auto-summary

do show ip route
do show ip route rip
do show ip protocols
```

## Manualna sumarizacia

```
int s0/0/0
 ip summary-address rip 192.168.1.0 255.255.255.0
```

## Pasivne rozhrania

Pasivne rozhrania budu prijimat updates, ale nebudu posielat  
Vhodne pri stub sietach

```
router rip
 passive-int g0/0
```

Alebo mozeme zvolit vsetky ako pasivne a iba niektore "odpasivnit"

```
router rip
 passive-int default
 no passive-int s0/0/0
```

## Redistribucia statickej cesty + default route

Ked potrebujeme cez RIP oznamovat staticku cestu

```
router rip
 redistribute static
```

Taktiez je mozne redistribuovat priamo pripojene cesty

```
router rip
 redistribute connected
```

Oznamovanie ostatnym ze "ja som default route"

```
router rip
 default-information originate
```

Overenie pomocout `do show ip route | begin Gateway`

## Discard route

Discard route je fiktivne virtualne rozhranie ktore zahodi vsetky pakety ktore mu pridu  
Vyuzitie pri vzniku smerovacej slucky  
Basically sa spravi staticka route na rozhranie `Null0`

```
ip route 192.168.1.0 255.255.255.0 Null0
```

## Autentifikacia v RIP

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

## RIPng

RIP pre IPv6

```
ipv6 unicast routing

int g0/0
 ipv6 rip NAZOV enable
int s0/0/0
 ipv6 rip NAZOV enable

do show ipv6 proto
```

> Note: NAZOV je akysi nazov procesu alebo co? Actually sa to _asi_ moze volat aj inak, napr. RIP1

### RIPng Default route

```
int s0/0/0
 ipv6 rip NAZOV default-information originate
```

> Note: Same as before, NAZOV je len nazov, napr. RIPt

# VLAN

Virtual LAN

VLANs allow administrator to segment networks based on factors such as function, team, or application, without regard for the pshysical location of the users or devices  
Each VLAN is considered a aseparate logical network  
Unicast, broadcast and multicast is forwarded and flooded onlyu to end devices within corresponding VLAN

Benefits

- Smaller broadcast domains - network is divided into parts
- Improved security - only users in the same VLAN can communicate together
- Improved IT efficiency - VLANs simplifies network management
- Reduced cost _(?)_
- Simpler project and application management

Access (pristupove) porty - medzi switchom a koncovym zariadenim - neznackuje sa  
Trunk porty - medzi switchom a inym sietovym zariadenim (switch, router)

## Trunk Protokoly

| 802.1Q                                       | ISL (Inter-Switch Link)                                            |
| -------------------------------------------- | ------------------------------------------------------------------ |
| Open source od IEEE                          | Cisco Proprietary                                                  |
| Pridana polozka TAG do Ethernet II. hlavicky | Znackovanie formou tunelovania = Enkapsulovany cely Ethernet ramec |
| Velkost +4B pre TAG (0x8100, CoS, DEI, VID)  | Velkost +Hlavicka (26B) +CRC (4B) = +30B                           |

### 802.1Q

Pridava sa TAG do Ethernet hlavicky:

- TPID (Tag Protocol Identifier)
  - Velkost **16b**
  - Hodnota `0x8100`
- Priority
  - Velkost **3b**
  - Dovoluje robit prioritne ramce v rozsahu 0 az 7
  - Niekedy sa toto prioritne znackovanie nazyva **CoS** (Class of Service)
- CFI
  - Canonical Format Indicator
  - Velkost **1b**
  - V minulosti pre Token Ring siete
  - V sucastnosti vyuzivany pre **Discard Eligible Indicator** = v pripade nutosti zahod ramec
- VID
  - Vlan ID
  - Velkost **12b**
  - Dovoluje vytvarat $2^{12} = 4096$ VLAN

Treba prepocitat FCS na konci Ethernetoveho ramca - zmena hlavicky  
Potencionalny problem - _Baby Jumbo frames_ - velkost sa zvacsi na 1522B

#### Vyhradene VLAN

- VLAN 0
  - Vyjadruje, ze TAG sluzi len na prenos informacie o priorite (CoS a DEI)
- VLAN 4095

  - Nemalo by sa nikdy objavit
  - Urcene pre vyrobcov na interne testovanie

- VLAN 1 (nie je vyhradena)
  - Default VLAN
  - Vzdy defaultne pritomna, neda sa zmazat
  - Su tu defaultne zaradene vsetky porty
  - Defaultne je zaroven aj Native VLAN, je dobre to zmenit

#### Proces komunikacie

Na jednom prepinaci

1. Prepinac prijme ramec
2. Pozre sa do smerovacej tabulky **iba na zaznamy pre danu vlan**
3. Ramec ostane nezmeneny a posle sa na vystupne rozhranie

Cez viac prepinacov

1. Prvy prepinac
   1. Prepinac prijme ramec
   2. Pozre sa do smerovacej tabulky **iba na zaznamy pre danu vlan**
   3. Prepinac vidi, ze vystupne rozhranie je oznacene ako **trunk port** _(napr. Fa0/1)_
   4. Prepinac vlozi identifikator (TAG)
   5. Prepne na prislusny interface
2. Druhy prepinac
   1. Prepinac prijme ramec
   2. Pozre sa do smerovacej tabulky **iba na zaznamy pre danu vlan**
   3. Odstrani TAG
   4. Prepne ramec

> Note: V pripade, ze by takato komunikacia nastala pre viac prepinacov (nie len 2), tak _Druhy prepinac_ **neodstranuje TAG**, iba ramec prepne dalej na dalsi prepinac

#### Nativna VLAN

Napr. v pripade ze cez trunk port pride ramec bez znacky  
Cisco pouziva pojem _Native VLAN_, iny vyrobcovia aj _Untagged VLAN_ alebo _Primary VLAN_  
Definuje sa pre kazdy port zvlast _(?)_

- Ak ma byt frame odoslany do nativnej vlan, tak znacku nedostane, resp. ak ju ma tak odstrani
- Ak pride frame bez znacky, bude zaradeny do nativnej vlan

Best practice:

- Nativna VLAN bude iba jedna _(pre dvojicu portov MUSI byt, medzi troma smerovacmi sa da zmenit na inu, ale preco by to niekto robil)_
- Zmenime, aby VLAN 1 (default VLAN) nebola nativna

## VLAN vo vacsich sietach

Ak mame `xy` prepinacov s `xyz` VLAN, tak konfiguracia moze byt narocna/zdlhava

Riesenim mozu byt viacere protokoly:

- VTP _(VLAN Trunking Protocol)_ - Cisco
- MVRP _(Multiple VLAN Registration Protocol)_ - IEEE - predchodca GVRP
- SNMP _(Simple Network Management Protocol)_

**Iba sirenie VLAN databazy!**

Protokoly spravia jeden smerovac ako server, ktory to bude posielat viac klientom  
Zarucuju identicky zoznam VLAN na prepinacoch v sieti  
Existuju este aj transparentne smerovace - rozosielaju VLAN databazu, ale neovplyvnuju ju svojou databazou - mozu mat inu konfiguraciu

## Typy VLAN

Pre Cisco smerovace

Range:

- 0 - 1005
  - _Normal range_
  - Podporovane vsade
  - Prenasane pomocou VTP, ak je pouzity
  - Su vzdy ulozene vo `flash:vlan.dat` - uchovane aj pri reboot aj pri zmazani `running-config`
  - Mozu byt ulozene aj v `running-config`
- 1006 - 4096
  - _Extended range_
  - Podporovane na novsich prepinacoch
  - Prenasane pomocou VTPv3 - len velmi nove prepinace
  - Su ulozene v `running-config`
  - Iba pri VTPv3 mozu byt aj vo `flash:vlan.dat`

Typy

- Default VLAN _(1)_
- Native VLAN _(napr. 100)_
- Access VLAN, resp. Data VLAN _(napr. 10, 20, ...)_
  - Konkretna jedna VLAN, do ktorej je zaradeny jeden access port
- Voice VLAN
  - Data z/do IP telefonu
- Management VLAN _(napr. 99)_

### Management VLAN

Pouziva za na management prepinaca  
Defaultna VLAN 1 by sa mala zmenit na inu

```
interface vlan 99
 ip add 192.168.1.2
```

## DTP - Dynamic Trunk Protocol

Interface moze byt na jednom z nasledovnych rezimov

```
# DTP, preferujem trunk, ale prisposobim sa druhej strane
switchport mode dynamic desirable

# DTP, preferujem access, ale prisposobim sa druhej strane (defaultne je toto)
switchport mode dynamic auto

# nie DTP, napevno trunk
switchport mode trunk

# nie DTP, napevno access
switchport mode access


# v netacade bolo aj tento prikaz, ale nie som si sure ze co robi
switchport nonegotiate
```

Ak su na obidvoch stranach rovnake mody tak nie je problem  
Situacia `trunk` a `access` = chyba - limited connectivity  
Situacia `dynamic desirable` a `dynamic auto` = trunk port (vyssia priorita)

Nejaka ta konfiguracia

```
interface range fa 0/7 - 8  # kofigurujem interfaces 7 a 8
 switchport trunk encapsulation dot1q   # pouzivam enkapsulaciu 802.1Q
 switchport mode trunk

do show interface trunk
do show interface f0/7 switchport
```

## Best practice

- Presunut nepouzivane porty do osobitnej nepouzivanej VLAN _(napr. 1000)_
  - Idealne tuto VLAN aj suspendovat a vypnut nepouzivane porty
  - Tzv. _Black Hole VLAN_
- Osobitna VLAN pre management
- Zmenit nativnu VLAN
- Pouzivat iba SSH
- Po konfiguracii a rozbehu portov vypnut DTP + neponechavat ziadne porty v dynamickom rezime
- Vyhnut sa akemukolvek pouzivaniu VLAN 1

## Prikazy

```
do show vlan brief
do show vlan [brief | id {vlan-id} | name {vlan-name} | summary]

do show int vlan 20
do show int f0/1 switchport

do show dtp int f0/1
```

```
int f0/1
 switchport mode access
 switchport access vlan 20

int f0/2
 switchport mode trunk
 switchport trunk native vlan 99
 switchport trunk allowed vlan 10,20,30,99

int f0/3
 switchport mode access
 switchport voice vlan 150


mls qos trust cos  # enable QoS
```

Zmena z Trunk na Access

```
int f0/1
 no switchport trunk allowed vlan
 no switchport trunk native vlan
 switchport mode access
```

Vytvorenie VLAN  
Tento priklad vytvori VLAN s cislom `10` a nazvom `studenti`

```
vlan 10
 name studenti
```

Zmazanie VLAN  
Ak zmazem VLAN, v ktorej bol nejaky port, tak port **nebude priradeny nikde**

```
no vlan 20
```

## VTP

VLAN Trunking Protocol  
S nazvom nema nic spolocne  
Redistribucia info o VLAN medzi smerovacmi

Cislo revizie - zvysuje sa kazdou modifikaciou databazy  
Pouzivaju sa iba trunk porty, Native VLAN

### Verzie

- VTPv1
- VTPv2
- VTPv3

| Feature                        | VTPv1    | VTPv2    | VTPv3                     |
| ------------------------------ | -------- | -------- | ------------------------- |
| VLAN Range                     | `1-1005` | `1-1005` | `1-4096`                  |
| Prunning                       | Yes      | Yes      | Yes                       |
| Multiple Spanning Tree Support | No       | No       | Yes                       |
| Token Ring VLAN Suport         | No       | Yes      | No                        |
| Transparent Mode Forwarding    | No       | Yes      | Yes                       |
| Primary server role            | No       | No       | Yes                       |
| Database propagation           | VLANs    | VLANs    | VLANs, Private VLANs, MST |
| Enhanced Authentication        | No       | No       | Yes                       |

### Mody

- Server
  - Moze modifikovat databazu
  - Spracovava a preposiela VTP spravy
- Client
  - Nemodifikuje databazu, iba sa z nej uci
  - Spracovava a preposiela VTP spravy
- Transparent
  - Nie je skutocnym clenom domeny
  - Preposiela spravy ale ignoruje ich obsah, vedie si svoju nezavislu databazu
  - Cislo revizie vzdy 0
- Off
  - Ignoruje a nepreposiela VTP spravy

### VTP domena

Cast siete, kde sa zdiela VLAN info  
Identifikovana spolocnym menom  
Switch moze byt iba v jednej domene

### Typy VTP sprav

- Summary advertisements
  - Server posiela sumarne VLAN info kazdych 5 minut alebo pri zmene databazy
  - Klient posle po zapnuti
  - Info o management domenach, VTP verzii, domenove meno, konf. revizne cislo, casova znacka
- Subnet advertisements
  - Nasledne za Summary advertisements, pri zmenach vo VLAN
- Advertisement requests
  - Jeden Advertisement requests per VLAN ID

Spravy su posielane na multicast adresu `01-00-0C-CC-CC-CC`  
Enkapsulovane do 802.1q formatu Ethernet LLC/SNAP ramca

### VTP Prunning

Zabranuje sireniu broadcastu do smerov, kde nie je potrebny  
Konfiguruje sa iba na serveroch

### VTP Prikazy

```
vtp domain MENO_DOMENY
vtp mode {client | server | transparent}
vtp password h@slisko
vtp version {1 | 2}

vtp prunning  # iba servery
```

```
do show vtp status
do show vtp counters
```

## Smerovanie medzi VLAN

### Router on Stick

```
int g0/0.10
    encapsulation dot1q 10
    ip address 192.168.10.1 255.255.255.0

int g0/0.20
    encapsulation dot1q 20
    ip address 192.168.20.1 255.255.255.0
```

### Multi-layer switching

Aktivacia L3 switchingu

```
ip routing
```

```
vlan 10,20,30
    exit

int vlan 10
    ip add 192.168.10.1 255.255.255.0

int vlan 20
    ip add 192.168.20.1 255.255.255.0

int vlan 30
    ip add 192.168.30.1 255.255.255.0
```

## Switch Boot Sequence

Postup ktory sa deje pri bootovani switcha

> Nevedel som kam to dat tak som to bachol sem. V netacade sa spominali switche hned na zaciatku

1. Power-on self-test (POST)
   - Program stored in ROM
   - Checks the CPU subsystem
   - Tests CPU, DRAM and portion of flash
2. Bootloader
3. Bootloader performs low-level CPU init
   - Initializes the CPU registers, which controls where physical memory is mapped, the quantity of memory and it's speed
4. Bootloader initializes the flash system on the system board
5. Bootloader locates and loads IOS image into memory and gives control of the switch to the IOS

```
S1(config)# boot system flash:/x2960-lanbasek9-mz.150-2.SE/c2960-lanbasek9-mz.150-2.SE.bin
```

## Switching forwarding methods

### Store-and-Forward

Switch nabufferuje **cely** frame az potom robi nieco dalej  
Skontroluje FCS, a ked je to v pohode tak posle frame dalej, inak ho dropne

### Cut-Through Switching

Nazyvany aj Fast-Forward switching  
Switch si precita len cielovu MAC adresu (**6 bit**) a hned posiela frame  
Je rychlejsie, ale mozu sa posielat chybne frames

#### Fragment free switching

Upravena verzia Cut-Through  
Precita prvych **64 bit**ov (In an Ethernet LAN, collision fragments are detected in the first 64 bytes)

# STP

Spanning Tree Protocol - `IEEE 802.1d`

```
I think that I shall never see
A graph more lovely than a tree.
A tree whose crucial preperty
Is loop-free connectivity.
A tree that must be sure to span
So packet can reach every LAN.
First, the root must be selecter.
By ID, it is elected.
Least-cost path from root are traced.
In the tree, these paths are placed.
A mesh is made by folks like me,
Then bridges find a spanning tree.

*Radia Perlman*
```

L2 protokol ktory zabranuje vzniku sluciek na L2

> Slucky, Broadcastove burky, nestabilita MAC/CAM tabulky, zacyklenie a zahltenie az kolaps siete

Komunikacia prebieha pomocou specialnych **BPDU** ramcov _(Bridge Protocol Data Unit)_

1. Volba Root Bridge (RB) - referencny bod stromu, je len jeden
2. Volba Root Ports - porty najblizsie k RB
3. Urcenie Designated (aktivne) a Non-Designated (blokovane) portov

BPDU okrem ineho obsahuju

- Root ID
- Cost of Path - vzialenost od RB
- Bridge ID - ID prepinaca ktory posledny manipuloval BPDU
- Port ID - Port, cez ktory bolo odoslane BPDU

**BID** - Bridge ID - jednoznacny identifikator Bridge-a  
Pozostava z Priority _(2B)_ a MAC Address _(6B)_

Podla priority sa urcuje Root - Bridge s najnizsim BID sa stane Root-om  
Pomalsi port = vyssie cislo Priority = mensia sanca byt Root

## Stavy portov

Ked sa pripoji/odblokuje port switch-a, nemoze hned forwardovat (nevie co je dalej - dalsi switch = slucka)

`Disabled port -> Inicialization -> Listening`

`Listening -> Blocking` alebo `Listening -> Learning -> Forwarding`

- **`Listening`**
  - Port pocuva a snazi sa zistit, aka je topologia a kde je RB
  - Prijima, spracovava a preposiela BPDU
  - Neprijima datove ramce, neuci sa MAC adresy
  - Zdrzi sa tu 15 sekund
  - Podla toho, ci by vznikla slucka alebo nie sa prepne do stavu `Blocking` alebo `Learning`
  - **`Blocking`**
    - Dostane sa sem, ak by forwardovanie sposobilo slucky
    - Iba prijima iba BPDU
    - Zdrzi sa tu 20 sekund
    - Po 20 sekundach ide zase `Init -> Listening`
  - **`Learning`**
    - Spracovava BPDU
    - Iba prijima datove ramce, z ktorych sa uci MAC adresy
    - Zostane tu 15 sekund, potom ide do `Forwarding`
    - **`Forwarding`**
      - Bezny beh portu

Problem - rychlost konvergenicie - 30 az 50 sekund - riesenie - _Rapid STP (RSTP)_ - radovo do 1 sekundy

| Port state | BPDU             | MAC address table | Forwarding data frames |
| ---------- | ---------------- | ----------------- | ---------------------- |
| Blocking   | Receive          | No update         | No                     |
| Listening  | Receive and Send | No update         | No                     |
| Learning   | Receive and Send | Updating table    | No                     |
| Forwarding | Receive and Send | Updating table    | Yes                    |
| _Disabled_ | -                | -                 | -                      |

## Role portov

Aku ulohu hra dany port v strome

Na RB su vsetky porty `Designated`  
Na Non-Root Bridge su porty `Root`, `Designated` alebo `Non-designated`

_`Designated port` je port, ktory ma prijima horsie BPDU ako to, ktore odosiela_  
_`Root port` je ten, ktory spomedzi vsetkych portov dostava najlepsie BPDU_

## Casovace

| Nazov         | Popis                                | Default hodnota | Mozne hodnoty  |
| ------------- | ------------------------------------ | --------------- | -------------- |
| Hello Time    | Cas medzi dvoma BPDU                 | 2 sekundy       | 1 az 10 sekund |
| Forward Delay | Cas v stave `Listening` a `Learning` | 15 sekund       | 4 az 30 sekund |
| Maximum Age   | Cas v stave `Blocking`               | 20 sekund       | 6 az 40 sekund |

## Rozhodovaci proces pouzivany pri porovnavani BPDU

Prepinace pri STP funguju na principe porovnavania ramcov - lepsi ramec = superior, horsi ramec = inferior

Porovnavaju sa v nasledovnom poradi (nizsie = lepsie)

---

1. Root Bridge ID
   1. Priority
   2. MAC Address
2. Root Path Cost
3. Sender Bridge ID
4. Sender Port ID
5. Receiver Port ID (len vynimocne)

---

Path cost - podla rychlosti linky (kabla)

| Speed    | STP Path Cost |
| -------- | ------------- |
| 10 Gbps  | 2             |
| 1 Gbps   | 4             |
| 100 Mbps | 19            |
| 10 Mbps  | 100           |

### Port Fast

Access port nemusi cakat na konvergenciu

## Prikazy

Overovacie prikazy

```
do show spanning-tree
do show spanning-tree summary
do show spanning-tree vlan VLAN_ID
```

Nastavenie/zrusenie STP pre danu VLAN

```
spanning-tree vlan VLAN_ID
no spanning-tree vlan VLAN_ID
```

Nastavenie priority pre switch

```
spanning-tree vlan VLAN_ID priority PRIORITY
```

Nastavenie switcha ako Root (zisti si prioritu u Root-a a znizi ju)

```
spanning-tree vlan VLAN_ID root primary
```

Nastavenie switcha ako zalozny Root

```
spanning-tree vlan VLAN_ID root secondary
```

Konfiguracia Port-fast (jeden port/vsetky porty)

```
int f0/1
 spanning-tree portfast

spanning-tree portfast default
```

## RSTP

Rapid STP

Ma len 3 stavy  
Zavadza `Edge port` - nieco ako Port Fast vlastnost  
Pouziva take iste BPDU, ale vyuziva mechanizmus _Proposal/Agreement_  
BPDU posiela kazdy switch (nie len Root ako v STP)  
Je spatne kompatibilny

### Zmena stavov portov

| STP Port State | RSTP Port State | Aktivny? | Uci sa MAC adresy? |
| -------------- | --------------- | -------- | ------------------ |
| Disabled       | Discarding      | Nie      | Nie                |
| Blocking       | Discarding      | Nie      | Nie                |
| Listening      | Discarding      | Ano      | Nie                |
| Learning       | Learning        | Ano      | Ano                |
| Forwarding     | Forwarding      | Ano      | Ano                |

### Zmena definicie portov

- Root
  - Iba na Non-Root Bridge
  - Najblizsi port k Root
- Designated
  - Odosiela lepsie BPDU ako prijima
- Alternate
  - Zaloha Root portu
  - Ked padne Root, snazi sa aktivovat Alternate
- Backup
  - Zaloha Designated portu
  - Ked padne Designated, snazi sa aktivovat Backup
- Edge
  - Po starom Port Fast
  - Access port, iba koncove stanice (nie dalsi switch)
  - Nepredpoklada sa zmena topo
  - Neflushuju sa CAM tabulky

### Nove typy liniek

#### Edge linky

Spaja koncove zariadenie so switchom  
Access port

#### Non-Edge linky

Medzi switchami  
Trunk port

Dalej sa deli na 2 kategorie

- Point-to-point
  - Full-duplex
- Shared Port
  - Half-duplex
  - V pripade ak je spojenie switch - hub
  - Konvergencia do 15 sekund
  - V modernych sietach takmer nepouzivane

### Proposal/Agreement mechanizmus

Mechanizmus na zrychlenie konvergencie  
Napr. situacia ked sa novy switch priamo napoji na RB (novy switch ma aj inu cestu k RB)  
RB posle novemu switchu **Proposal** - ponukne mu cestu  
Novy switch to moze prijat (v tejto situacii prijme) - **Agreement**  
Pri tomto procese sa docasne zablokuju ostatne porty (ak prijmem Proposal tak ostane block, inak sa naspat aktivuje)

#### Konfiguracia

Zapnutie Rapid PVST+ _(Rapid Per-Vlan Spanning Tree +)_

```
spanning-tree mode rapid-pvst
```

Ostatne prikazy su rovnake

```
spanning-tree vlan VLAN_ID priority PRIORITY
spanning-tree vlan VLAN_ID root primary
spanning-tree vlan VLAN_ID root secondary

int f0/1
 spanning-tree portfast
 spanning-tree link-type point-to-point
```

Show prikazy

```
do show spanning-tree
do show spanning-tree vlan VLAN_ID
do show spanning-tree detail
do show spanning-tree active
```

# ACL

Access control list
_"ACLs are among the most commonly used features of Cisco IOS software"_  
Najcastejsie nasadenie ako pravidla riadenia IP prevadzky. Pouzite aj vsade kde potrebna nejaka klasifikacia alebo identifikacia toku

Sklada sa z podmienok - ACL Statements = Access Control Entries - ACEs  
Kazdy zaznam obsahuje testovaciu podmienku a akciu (`permit` alebo `deny`)  
Existuje este jedna akcia okrem `permit` a `deny` - `remark` - sluzi len ako poznamka/comment a nevykonava ziadnu akciu

## Co sa riesi v ACL

Akoze co sa da specifikovat

- Smer toku dat
  - Odkial - source/sender
  - Kam - destination/receiver
- Kto
  - Jednotlivec/Skupina/Vsetci
  - Rozlisuje sa pomocou parametrov
    - IP adresa
    - Typ protokolu (IP, ICMT, TCP, UDP )
    - Sluzba (cislo portu)

## Nasadenie ACL

- Jeden ACL per **interface**
- Jeden ACL per **protokol**
- Jeden ACL per **smer**

Cize napr. ak jedno rozhranie podporuje aj IPv4 aj IPv6, tak budeme mat

- Jeden ACL pre IPv4 pre smer Inbound
- Jeden ACL pre IPv4 pre smer Outbound
- Jeden ACL pre IPv6 pre smer Inbound
- Jeden ACL pre IPv6 pre smer Outbound

Samozrejme ACL nemusi byt nasadeny vsade, napr. moze byt len IPv4 Inbound

## Ako pracuje ACL

- Zoznam sa prehladava postupne jeden zaznam po druhom - preto specificke zaznamy treba na zaciatok, vseobecne na koniec
- Ak sa najde zhoda v podmienke
  - Pozrie ci je `permit` alebo `deny`
    - Ak je `permit` tak sa pokracuje v normalnom smerovani
    - Ak je `deny` tak sa paket zahodi
  - **Po prvej zhode sa uz dalej nepokracuje v hladani v ACL!**
- Ak sa v celom zozname nenajde zhoda v podmienke
  - Na konci je implicitna default akcia (nie je viditelna) `deny any`, ktora zahodi vsekty pakety

Pri Inbound sa kontroluje ACL pred smerovanim  
Pri Outbound sa kontroluje ACL po smerovani  
Cize v podstate: Pride paket - ACL na vstupnom rozhrani - Smerovanie - ACL na vystupnom rozhrani

## Typy ACL

### Standard ACL

Cislo `1-99` alebo `1300-1999`

- Povoluje/Zakazuje cely protokolovy stack
  - Napr. zakaze celu IPv4
- Kontroluje iba source IP add
- Nevie specifikovat sluzby (port)

### Extended ACL

Cislo `100-199` alebo `2000-2699`

- Specifikuje aj source aj destination IP add
- Specifikuje aj source aj destination port

## Cislovane a Pomenovane ACL

ACL sa da bud pomenovat alebo ocislovat

### Cislovane

- Standard ACL
  - `1-99`
  - `1300-1999`
- Extended ACL
  - `100-199`
  - `2000-2699`

Nevyhoda (len na starsich IOS) - nedaju sa mazat podmienky, pridavat sa daju len na koniec

### Pomenovane

- Vacsinou uppercase nazov
- Prehladnejsie - meno moze napovedat o co ide
- Daju sa mazat aj pridavat hociktore zaznamy

## Prikazy

### Standardne cislovane ACL

```
access-list CISLO [deny|permit|remark] TESTOVACIA_PODMIENKA [WILDCARD_MASK] [log]

do show access-lists 1
```

Ak specifikujeme _log_ na konci, tak vzdy ked sa paket zachyti na tomto pravidle, tak sa vypise nejaky vypis na konzolu (raz za 5 minut)

## Wildcard mask

_"Opacna subnet mask"_  
_"Ma sa dany bit ignorovat?"_

Moze vyzerat napr. takto: `0.0.0.255`

- 0 = kontroluj zhodu
- 1 = ignoruj zhodu

### Priklady

Mame siet **172.16.10.0/24** a chceme specifikovat to co je v tabulke  
Aka bude testovacia podmienka a wildcard maska

> Nie som si sure ze to tak ma byt

| Co chceme specifikovat | Poznamka                               | Testovacia podmienka | Wildcard mask (posledny oktet)            |
| ---------------------- | -------------------------------------- | -------------------- | ----------------------------------------- |
| Prvu polovicu hostov   | `172.16.10.0` az <br>`172.16.10.127`   | `172.16.10.0`        | bin `1000 0000` <br>dec `128`             |
| Druhu polovicu hostov  | `172.16.10.128` az <br>`172.16.10.255` | `172.16.10.255`      | bin `1000 0000` <br>dec `128`             |
| Parne IP adresy        |                                        | `172.16.10.0`        | bin `1111 1110` <br>dec `254`             |
| Neparne IP adresy      |                                        | `172.16.10.1`        | bin `1111 1110` <br>dec `254`             |
| Presne jeden host      |                                        | Napr. `172.16.10.1`  | bin `0000 0000` <br>dec `0`               |
| Cokolvek               | Pustit hocikoho                        | Napr. `172.16.10.1`  | bin `1111 1111` <br>dec `255.255.255.255` |

| Co chceme specifikovat                                 | Testovacia podmienka | Wildcard mask                                      |
| ------------------------------------------------------ | -------------------- | -------------------------------------------------- |
| Vsetci hostia <br>`172.16.0.0/24` az <br>`172.16.15.0` | `172.16.0.0`         | bin `0.0.0000 1111.1111 1111` <br>dec `0.0.15.255` |
| Vsetci hostia <br>`172.16.32.0` az <br>`172.16.47.0`   | `172.16.32.0`        | bin `0.0.0001 1111.1111 1111`                      |

### Klucove slova `host` a `any`

Ked specifikujeme `any` tak pustame vsetkych dnu - wildcard mask = `255.255.255.255`  
`host` je presne naopak - specifikujeme len jedneho hosta - wildcard mask = `0.0.0.0`

```
access-list 2 permit 192.168.10.123 0.0.0.0
access-list 2 permit host 192.168.10.123

access-list 2 permit 192.168.20.123 255.255.255.255  # ip moze byt hocico
access-list 2 permit any
```

## Priradenie ACL na rozhranie

```
int g0/0
 ip access-group {ACL_number | ACL_name} {in | out}
 ip access-group 2 out

line vty 0 4
 ip access-class {ACL_number | ACL_name} {in | out}
 ip access-class 3 in
```

## Editovanie ACL

Iba v novsich IOS

```
do show access-list 1  # vsimneme si cislo zaznamu

ip access-list standard 1
 no 10  # vymaze zaznam 10
 10 deny host 192.168.1.10  # novy zaznam na index 10
```

## Pozor na mazanie ACL

Ked sa zmaze ACL, ktory je aktualne aplikovany na nejakom rozhrani  
Vymaze sa len obsah ACL a na konci zostane `deny any`  
Tym padom sa znefunkcni kazdy interface na ktorom je aplikovane dane ACL

Treba najprv zrusit ACL z daneho rohrania, az potom vymazat ACL

```
int f0/0/0
 no ip access-group 2

no access-list 2
```

## Konfiguracia pomenovaneho ACL

```
access-list 2 permit host 192.168.1.10  # kofiguracia cislovaneho

# vytvorenie
ip access-list {standard | extended} NAZOV
 {permit | deny | remark} TESTOVACIA_PODMIENKA WILDCARD_MASK [log]

# aplikacia na interface
int f0/0/0
 ip access-group NAZOV [in | out]
```

---

```
do show access-list
do show access-list 1

do show ip access-list

do show run | include access-list
do show run | include access-list 1
```

Reset pocitadiel (pri specifikovani `log` na konci)

```
do show access-list
do clear access-list counters 1
```

## Konfiguracia extended ACL

```
access-list CISLO {deny | permit | remark} PROTOKOL {SOURCE SOURCE_WILDCARD} [OPERATOR PORT [PORT_NUMBER | WILDCARD]] {DESTINATION DESTINATION_WILDCARD} [OPERATOR PORT [PORT_NUMBER | WILDCARD]]

access-list 101 remark Povol HTTP
access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 80
access-list 101 remark Povol HTTPS
access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 443

access-list 101 remark Nejaky pokus
access-list 101 permit ip 192.168.1.0 0.0.0.255 lt 100 any ip
```

## IPv6 ACL

Velmi podobne ako IPv4, hlavne rozdiely:

- Prikaz pri aplikovani - `ipv6 traffic-filter`
- Nepouziva wildcard mask, ale prefix length
- Niektore podmienky navyse
  - `permit icmp any any nd-na`
  - `permit icmp any any nd-ns`
- Iba pomenovane ACL's

```
ipv6 access-list NAZOV_ACL
 {deny | permit} PROTOCOL {SOURCE_IPV6_ADD/PREFIX_LENGTH | any | host SOURCE_IPV6_ADD} [OPERATOR [PORT_NUMBER]] {DEST_IPV6_ADD/PREFIX_LENGTH | any | host DEST_IPV6_ADD} [OPERATOR [PORT_NUMBER]]

int s0/0/0
 ipv6 traffic-filter NAZOV_ACL in
```

```
ipv6 access-list PRIKLAD1
 deny ipv6 2001:db8:cafe:30::/64 any
 deny tcp any 2001:db8:cafe:11::/64 eq ftp
 deny tcp any 2001:db8:cafe:11::/64 eq ftp-data
 permit ipv6 any any

int s0/0/0
 ipv6 traffic-filter PRIKLAD1 in
```

# Etherchannel & FHRP

First hop redundancy, agregacia portov  
Treba zabezpecit, ze ak jeden router padne, tak ho nahradi druhy + load balancing  
Pokusy o riesenie cez Proxy ARP, ICMP Router Discovery Protocol (IRDP) alebo v OS Hosta - neefektivne

#### Riesenie pomocou Virtual Router

2+ smerovace sa tvaria ako jeden  
Tento Virtual router ma svoju vMAC a vIP (ktora je default gateway)  
Iba jeden z realnych routerov bude nositelom vMAC a vIP - Primary/Active/Master  
Ostatne - Secondary/Backup/Slave - kontroluju ci existuje Primary - ci odpoveda na spravy - inak si prevezmu danu vMAC a vIP  
Z pohladu koncovej stanice sa MAC a IP nezmenila

#### Riesenia pre FHRP

- ICMP Router Discovery Protocol (IRDP)
  - ICMP spravy Router Advertisement + Router Solicitation
- Hot Standby Router Protocol (HSRP)
  - Cisco
  - IPv4 - HSRPv1
  - IPv6 - HSRPv2
- Gateway Load Balancing Protocol (GLBP)
  - Cisco
  - Vylepsenie oproti HSRP
- Virtual Router Redundancy Protocol (VRRP)
  - IETF

## HSRP (Hot Standby Router Protocol)

Jeden Virtual Router  
Posielanie paketov cez Multicast

- `224.0.0.2` pri `v1`
- `224.0.0.102` pri `v3` _(?)_, resp. `FE02::66` pre IPv6

Pocet grup: 256, vo `v3` az 4096, realne 16

### Rozdelenie Routerov v HSRP

- Active router
  - V grupe vzdy iba jeden
  - Nositel identity virtualneho routera (vMAC, vIP)
  - Obsluhuje vsetky pakety
  - Bud prvy nabootovany alebo (pri preempcii) ten s najvyssou prioritou
- Standby router
  - V grupe vzdy najviac jeden
  - Zalozny pre Active router - zastupuje ho ked Active zomrie - _"druhy v poradi"_
  - Bud druhy nabootovany alebo (pri preempcii) ten s druhou najvyssou prioritou
- Other routers
  - Vsetky ostatne
  - Vedia prejst do Standby a nasledne Active

Vsekty spolu tvoria Virtual Router

### HSRP stavy

| State   | Definition                                                                                                                  |
| ------- | --------------------------------------------------------------------------------------------------------------------------- |
| Initial | Hned po zapnuti/nakonfigurovani                                                                                             |
| Listen  | Router pozna vMAC a vIP ale nie je ani Active ani Standby <br>Pocuva na hello spravy                                        |
| Speak   | Periodicky posiela hello spravy <br>Zucastnuje sa na volbach o Active/Standby <br>Router sem nemoze vstupit pokial nema vIP |
| Standby | Zalozny Active                                                                                                              |
| Active  | Forwardinguje pakety <br>Posiela hello messages                                                                             |

### Konfiguracia HSRP

Config. pre 2 routery a 2 VLANs

```
# ROUTER 1
# grupa cislo 1
int f0/0.1
 encapsulation dot1q 1
 ip add 192.168.1.101 255.255.255.0
 standby version 2
 standby 1 priority 150
 standby 1 ip 192.168.1.1
 standby 1 preempt

# grupa cislo 2
int f0/0.2
 encapsulation dot1q 2
 ip add 192.168.2.101 255.255.255.0
 standby version 2
 standby 2 ip 192.168.2.1
 standby 2 preempt

router rip
 network 10.0.0.0
 network 192.168.1.0
 network 192.168.2.0
```

```
# ROUTER 2
# grupa cislo 1
int f0/0.1
 encapsulation dot1q 1
 ip add 192.168.1.102 255.255.255.0
 standby version 2
 standby 1 ip 192.168.1.1
 standby 1 preempt

# grupa cislo 2
int f0/0.2
 encapsulation dot1q 2
 ip add 192.168.2.102 255.255.255.0
 standby version 2
 standby 2 priority 150
 standby 2 ip 192.168.2.1
 standby 2 preempt

router rip
 network 10.0.0.0
 network 192.168.1.0
 network 192.168.2.0
```

#### Show prikazy

```
do show standby
do show standby brief

debug standby packets
debug standby packets terse
```

Pri `do show standby brief` znamena pismenko `P`, ze je nastavena preempcia

## Agregacia liniek

Link Aggregation, EtherChannel, ChannelBonding
Zdruzovanie liniek

### EtherChannel

Switch-Switch, Switch-Router, Switch-Server  
Mozeme zdruzovat od 2 do 8 fyzickych portov do jedneho logickeho  
Vsetky fyzicke rozhrania musia mat rovnaku rychlost, duplex a VLAN info (zoznam povolenych VLANs)

Load balancing moze byt zalozeny na nasledovnych kriteriach

- `src-mac`
- `dst-mac`
- `src-dst-mac`
- `src-ip`
- `dst-ip`
- **`src-dst-ip`** - default
- `src-port`
- `dst-port`
- `src-dst-port`

### Implementacie agregacie linky

Ako pri vsetkom je mozne dynamicky a staticky

#### Dynamicky

- PAgP (Port Aggregation Protocol)
  - Cisco
  - Spravy kazdych 30 sekund
  - Mody
    - **Auto** - pasivny stav, odpoveda na ziadosti ale nevytvara ich
    - **Desirable** - aktivne ziada o vytvorenie kanala posielanim PAgP paketov
- LACP (Link Aggregation Protocol)
  - IEEE 802.3ad
  - Mody
    - **Passive** - to iste ako PAgP Auto
    - **Active** - to iste ako PAgP Desirable, posila LACP pakety

Mozne kombinacie

- Auto + Auto = No channel
- Auto + Desirable = Channel
- Desirable + Desirable = Channel
- Manualne ON + Manualne ON = Channel
- Manualne OFF + hocico = No channel

##### PAgP konfiguracia

```
int range f0/1 - f0/2
 channel-group GROUP_NUMBER mode MODE
 [channel-port {pagp | lacp}]

do show ip int b

int port-channel GROUP_NUMBER
 # dalsia konfiguracia ako normalne
 sw mode access
 sw access vlan 10
 ...

port-channel load-balance TYPE
do show etherchannel load-balance
```

Show prikazy

```
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

#### Staticky

# DHCP

Historicky

1. RARP - Reverse Address Resolution Protocol
   - Klient iba ziska adresu, treba nakonfigurovat RARP table
2. BOOTP - BOOTstrap Protocol
   - Obmedzeny pocet parametrov oproti DHCP, 2-fazovy konfig. proces
   - Klient nerobi rebind/renew konfig. so serverom, okrem restartu
3. DHCP - Dynamic Host configuration Protocol
   - Stanici pridelena adresa len kym komunikuje, pri novom prihlaseni nova
   - Siroka mnozina konfig. parametrov, 1-fazovy konfig
   - Nepotrebuje restart, rebind/renew sa deje automaticky po casovom intervale

## DHCP Komponenty

- DHCP klient
  - Ziada o konfig. parametre DHCP server cez L2 broadcast
- DHCP server
  - Moze byt dedikovany server alebo smerovac
  - Spravuje IP adresnu mnozinu a ine konfig. parametre
  - Prideluje ich na poziadanie DHCP klientom
- Relay Agent - umoznuje prechod DHCP ziadosti cez L3 zariadenia - iba v pripadoch ked je to potrebne

## Alokacia IP adresy cez DHCP

- Dynamicka alokacia
  - Prideli IP adresu hostovi na specifikovane casove obdobie (lease - limited)
  - Potom nastava uvolnenie adresy alebo obnovenie "prenajmu"
- Automaticka alokacia
  - Prideli permanentnu staticku IP adresu (lease - unlimited)
- Manualna alokacia
  - Vyzaduje konfig. servera
  - Prideli vzdy rovnaku IP adresu stanici (na zaklade MAC), ktoru klient nevracia

## DHCP spravy

DORA Process - **D**iscover, **O**ffer, **R**equest, **A**cknowledgement

1. DHCP Discover (L2 broadcast)
2. DHCP Offer (L2 Unicast)
3. DHCP Request (L2 Broadcast)
4. DHCP ACK (L2 Unicast)

Obsah sprav

- CIADR - Client IP address
- Mask
- GIADR - Gatway IP address
- CHADR - Client Hardware address (MAC)

## Konfiguracia DHCP servera na Cisco router

- Spustenie sluzby a pomenovanie konfiguracie (na jednom smerovaci moze byt viac rozsahov)
- Nastavenie parametrov sluzby
  - IP rozsah
  - Default gateway address
  - DNS server address
  - Ine parametre - celkovo ich moze byt az okolo 50

```
ip dhcp pool MOJ_DHCP
 network 192.168.1.0 255.255.255.128
 default-router 192.168.1.1
 dns-server 195.146.132.59
 ?

ip dhpc excluded-address 192.168.1.1 192.168.1.16

do show ip dhcp binding
do show ip dhcp server statistics
do show ip dhcp pool
do show ip dhcp conflict
do show run | begin dhcp
do show run | include no service dhcp  # ci nie je vypnute dhcp

do clear ip dhcp binding *

do debug ip dhcp server events
```

Z pohladu klienta (Cisco router) (neoporuca sa)

```
int g0/0
 ip add dhcp

do show ip int g0/0
```

Windows

```
ipconfig /?
ipconfig /release
ipconfig /renew
```

## Relay agent

DHCP sa siri broadcastom  
Co aj je DHCP server inde v sieti, az za smerovacom?  
Treba spravit Relay Agenta, ktory zabali L2 broadcast na L3 unicast a posle paket na server

Na default gateway rozhrani sa da tento prikaz

```
int g0/0
 ip helper-address IP_ADRESA_DHCP_SERVERA
```

Cez toto sa prenasaju aj ine sluzby (vsetko UDP)

- Port 37: Time
- Port 49: TACACS
- Port 53: DNS
- Port 67: DHCP/BOOTP server
- Port 68: DHCP/BOOTP client
- Port 69: TFTP
- Port 137: NetBIOS name service
- Port 138: NetBIOS datagram service

`ip forward-protocol`

---

Pouzitie ACL, ked je debug moc obsiahly

```
access-list 100 permit ip host 0.0.0.0 host 255.255.255.255
debug ip packet detail 100
```

## DHCPv6

### IPv6 opakovanie

Global Unicast - Dynamicka konfiguracia IPv6 adresy

- SLAAC - Stateless address autoconfiguration
  - RA: Poskytnem ti vsetko co potrebujes (Prefix, Mask, DNS)
- SLAAC + DHCPv6 (stateless)
  - RA: Poskytnem ti nieco (Prefix, Mask), ale o ostatne poziadaj DHCPv6 server
- DHCPv6 (stateful)

  - RA: Neviem ti pomoct, poziadaj DHCPv6

- Host posle ziadost o svoje adresne info vsetkym routerom
  - ICMPv6 - **Router Solicitation (RS)** spravu na `IPv6 all-routers multicast` (IPv6 nepouziva broadcast)
- Router posiela kazdych 200 sekund (alebo ako odpoved na otazku hosta) **Router Advertisement (RA)** spravy
  - Tiez cez ICMPv6 - ciel `IPv6 all-nodes multicast`
  - Moze ponuknut jednu z 3 moznosti (SLAAC, ...)

---

- SLAAC
  - Host posle spravu `ICMPv6 type 133` (RS)
    - Source IP - `::`
    - Dest. IP - IPv6 all-routers multicast = `ff02::2`
  - Router nasledne posle `RA option 1`
    - Network Prefix, Prefix Length, DNS addresses, Default Gateway
    - Moze poslat Lifetime, ...
    - Source IP - Router Link-Local address
    - Dest IP - IPv6 all-nodes multicast = `ff02::1`
  - Host si urci Interface ID (poslednych 64 bitov IPv6 adresy) a nasledne ho prilepi k Prefixu od routera
    - Ma na to 2 moznosti
      - Modified EUI-64 - z MAC adresy (`prva polovica + FFFE + druha polovica`) - Cisco zariadenia
      - Nahodne 64 bit cislo - Windows
    - Vysledok je 128 bit IPv6 adresa
  - Duplicate Address Detection (DAD)
    - _"Nema uz niekto takuto adresu?"_
    - Dest IP - IPv6 solicited-node multicast
      - `ff02::1:ff:X/104`, kde `X` = spodnych 24 bitov IP adresy, ktoru si idem nastavovat
- SLAAC + DHCPv6
  - Prvy krok rovnaky
  - Router posle `RA option 2`
    - Network Prefix, Prefix Length, Default Gateway
    - DNS treba poziadat DHCPv6 server
  - Treti krok rovnaky
- DHCPv6
  - Prvy krok rovnaky
  - Router posle `RA option 3`
    - Odkaze hosta na DHCPv6 server

### Konfiguracia DHCPv6

- `M` bit
  - "Managed address configuration" flag
  - Povie klientovi, aby poziadal DHCPv6 server o info
- `O` bit
  - "Other configuration" flag
  - povie klientovi, aby poziadal DHCPv6 server o informacie okrem Prefixu a Gateway (DNS, ...)

|         |                  |
| ------- | ---------------- |
| MO = 00 | SLAAC            |
| MO = 01 | stateless DHCPv6 |
| MO = 10 | statefull DHCPv6 |

#### Stateless server

MO = 01

```
ipv6 unicast routing
ipv6 dhcp pool NAZOV_DHCPV6
 dns-server 2001:db8:cafe:aaaa::5
 domain-name example.com

int g0/0
 ipv6 add 2001:db8:cafe:1::1/64
 ipv6 dhcp server NAZOV_DHCPV6
 ipv6 nd other-config-flag
```

#### Stateless klient

```
int g0/0
 ipv6 enable
 ipv6 address autoconfig
```

#### Statefull server

MO = 10

```
ipv6 unicast-routing
ipv6 dhcp pool NAZOV
 address prefix 2001:db8:cafe:1::/64 lifetime infinite infinite
 dns-server 2001:db8:cafe:aaaa::5
 domain-name example.com

ing g0/0
 ipv6 address 2001:db8:cafe:1::1/64
 ipv6 dhcp server NAZOV
 ipv6 nd managed-config-flag
```

#### Statefull Relay Agent

```
int g0/0
 ipv6 dhcp relay destination DHCP_SERVER_IP_ADD
```

# NAT

Network Address Translation  
(okrem ineho) preklada privatne IP adresy na verejne a naopak

Privatne adresne priestory:

| Class | Address range    | First         | Last              |
| ----- | ---------------- | ------------- | ----------------- |
| A     | `10.0.0.0/8`     | `10.0.0.0`    | `10.255.255.255`  |
| B     | `172.16.0.0/12`  | `172.16.0.0`  | `172.31.255.255`  |
| C     | `192.168.0.0/16` | `192.168.0.0` | `192.168.255.255` |

## Pojmy

Border Gatway Router

- NAT zariadenie
- Pracuje typicky na hranici stub siete
  - Stub siet ma iba jeden vstup/vystup do/z nej

Inside Network

- Vnutorna privatna siet (firma)
- Pri komunikacii mimo je objektom NAT-ovania
- Pri vnutornej komunikacii sa neNAT-uje

Outside Network

- Vonkajsia siet

---

- Inside Local Address
  - Pridelena vo vnutri siete
  - Podla RFC 1918 typicky privatna
- Inside Global Address
  - Platna verejna IP adresa od ISP
  - Na tuto adresu bude prekladana `Inside Local Address`
- Outside Global Address
  - Platna verejna IP adresa koncoveho zariadenia
  - Tuto adresu vidi odosielatel z vnutornej siete
- Outside Local Address
  - Lokalna adresa pridelena zariadeniu vo vonkajsej sieti
  - Ak ciel nepouziva NAT, tak je zhodna s `Outside Global Address`

## Sposoby NAT mapovania

### Staticke mapovanie

`1:1` tzv "one-to-one mapping"  
Preklada jednu privatnu na jednu verejnu adresu  
Potrebne ak treba zabezpecit pristup na stanicu (HTTP server) z internetu, a stanica je za NAT

| Inside local address | Inside global address |
| -------------------- | --------------------- |
| `192.168.1.10`       | `209.165.200.226`     |
| `192.168.1.11`       | `209.165.200.227`     |
| `192.168.1.12`       | `209.165.200.228`     |

### Dynamicke mapovanie

`n:n`
NAT ma pristupny rozsah verejnych adries - IP address pool  
NAT riadi preklad pridelovanim neobsadenych verejnych IP adries z poolu

| Inside local address | Inside global address |
| -------------------- | --------------------- |
| `192.168.1.10`       | `209.165.200.226`     |
| \*\*Available\*\*    | `209.165.200.227`     |
| \*\*Available\*\*    | `209.165.200.228`     |
| \*\*Available\*\*    | `209.165.200.229`     |
| \*\*Available\*\*    | `209.165.200.230`     |

### PAT (Protocol Address Translation)

`n:m`, kde `1 <= m < n`  
Tzv. NAT overloading

Napr. ak mam pridelenu iba jednu verejnu IP  
PAT mapuje viacere adresy na jednu verejnu a rozlisuje ich pomocou cisla portu (L4) (16 bit)

Napr. ak mam pridelenych iba niekolko verejnych IP adries, a tento pocet je mensi ako pocet zariadeni  
Taktiez sa odlisuju portom

Pouzivaju sa dynamicke (privatne) porty: `49152` - `65535`

| Inside Local        | Inside Global          | Outside Global       | Outside Local        |
| ------------------- | ---------------------- | -------------------- | -------------------- |
| `192.168.1.10:1555` | `209.165.200.226:1555` | `231.165.201.1:80`   | `231.165.201.1:80`   |
| `12.168.1.10:2345`  | `209.165.200.226:2345` | `231.165.208.129:80` | `231.165.208.129:80` |

## Konfiguracia NAT

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

Show prikazy

```
do show ip nat translations
do show ip nat statistics

do show run | include nat
do debug ip nat
```

Vymazanie NAT tabulky

```
clear ip nat translations *
```

## Konfiguracia PAT (PNAT)

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

## Port Forwarding

...je to vlastne staticky NAT preklad ktory specifikuje aj cislo portu

```
ip nat inside source static tcp 192.168.10.254 80 209.165.200.255 8080
```

## NAT64

Preklada IPv6 adresy na IPv4 adresy  
Ak nejaka cast pouziva vyhradne IPv6 a druha nepouziva IPv6

# Security

AAA = Authentication, Autorization, Accounting

- Local AAA
- Server-based AAA

Moznosti serverovej autentifikacie a autorizacie

- RADIUS (Remote Authentication Dial-In User Service)
  - UDP porty IANA 1812 (auth) / 1813 (account)
  - UDP porty Cisco 1645 (auth) / 1646 (account)
  - Sifrovana len cast spravy s heslom
  - Kombinuje autentifikaciu a autorizaciu
  - Ponuka robustne account funkcie
  - Podporuje remote access riesenia (dot1x)
- TACACS (Terminal Access Controller Access Control System+)
  - Robustne (heavy riesenie)
  - Zabezpecuje cele pripojenie sifrovanim
  - TCP port 49
  - Separuje autentifikaciu a autorizaciu
  - Umoznuje kombinovat rozne metody

```
aaa new-model
username lastresort password h@slisko
radius server SERVER-R
    add ipv4 192.168.1.100 auth-port 1812 acct-port 1813
    key RADIUS-heslisk0

tacacs server SERVER-T
    add ipv4 192.168.1.101
    single-connection
    key TACACS-hesl1sko

aaa auth login MY_AUTH_RAD+TAC group radius group tacacs+ local-case

line vty 0 15
    login auth MY_AUTH_RAD+TAC
```

## L2 utoky

| Nazov utoku                            | Popis                                                                                                                                                                                                     | Mitigation                                                                                                        |
| -------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| MAC Address Flooding                   | Ramce s roznymi neplatnymi MAC adresami zaplavia Switch. Vycerpa sa miesto v CAM tabulke. Novy pouzivatel sa nemoze dostat do tabulky. Zaznam validneho pouzivatela nie je v tabulke - ramce su floodnute | Port security. MAC address VLAN                                                                                   |
| VLAN Hopping                           | Zmena VLAN ID v enkapsulovanom pakete (doble tagging). Utocnik moze posielat/prijimat pakety z roznych VLAN a obist tak L3 security                                                                       | Sprisnit konfiguraciu trunkov _(?)_ a nonegotiate state nepouzitych portov. Dat nepouzite porty do spolocnej VLAN |
| Attacks betweed devices on common VLAN | Jednotlive zariadenia na spolocnej VLAN mozu potrebovat ochranu pred sebou. Obzvlast na IPS segmentoch, ktore podporuju zariadenia roznych zakaznikov                                                     | Implementacia private VLANs (PLVAN)                                                                               |
| DHCP starvation + DHCP spoofing        | Utocnik moze vycerpat adresny priestor dostupny pre DHCP server na nejaky cas. Taktiez sa moze vydavat za DHCP server v man-in-the-middle utokoch                                                         | DHCP snooping                                                                                                     |
| MAC spoofing                           | Utocnik spoofuje (vydava sa za) MAC adresu alebo validneho hosta ktory je momentalne v CAM tabulke. Switch potom switchuje ramce urcene validnemu hostovi na utocnika                                     | DHCP snooping. Port security                                                                                      |
| ARP spoofing                           | Utocnik vytvara APR odpovede urcene validnym hostom. Nasledne sa MAC adresa utocnika stane destination address v ramci vygenerovanom validnym hostom                                                      | Dynamic ARP inspection (DAI), DHCP snooping, Port security                                                        |
| STP Compromises                        | Utocnik sa vydava za root bridge. Ak je uspesny, moze vidiet viac ramcov                                                                                                                                  | Proaktivne konfigurovat primarny a backup root devices. Enable root, BPDU guard or filter                         |
| CDP manipulation                       | Data v CDP ramcoch su plain-text a neautentifikovane. Mozu byt odchytene a moze byt prezradena topologia siete                                                                                            | Vypnut CDP na vsetkych portoch kde nie je zamerne pouzivane                                                       |
| SSH and Telnet attacks                 | Telnet pakety su plain text. SSHv1 ma problemy s bezpecnostou                                                                                                                                             | Pouzivat SSHv2. Pouzivat telnet spolu s VTY ACL                                                                   |

Ochrana voci MAC flooding - Port security

- Umoznuje na porte
  - Obmedzit pocet zariadeni ktore mozu byt pripojene k jednemu rozhraniu prepinaca (default 1)
  - Definovat zoznam bezpecnych MAC adries
  - Definovat co sa stane ak dojde k poruseniu niektoreho z bezpecnostnych pravidiel

Ktore MAC adresy su bezpecne

- Static secure MAC
  - Manualne nakonfigurovana
  - V konfiguracii aj v CAM tabulke
  - Po restarte sa nacita z ulozenej konfiguracie
- Dynamic secure MAC (default)
  - Dynamicky z CAM tabulky
  - Len v CAM tabulke
- Sticky route
  - Hybrid medzi static a dynamic
  - Ziskava sa dynamicky ale uklada sa do config

Pri prekroceni poctu max MAC adries - violation modes

| Violation Mode     | Forwards traffic | Sends Syslog Message | Increase violation counter | Shuts down port |
| ------------------ | ---------------- | -------------------- | -------------------------- | --------------- |
| Protect            | No               | No                   | No                         | No              |
| Restrict           | No               | Yes                  | Yes                        | No              |
| Shutdown (default) | No               | Yes                  | Yes                        | Yes             |

```
int f0/2
    switchport mode access
    # nepovinne ale odporucane prikazyDy
    switchport port-security maximum 5
    switchport port-security mac-address aaaa.bbbb.cccc
    switchport port-security violation restrict
    switchport port-security aging time 10 type absolute
    switchport port-security

do show port-security
do show port-security int f0/2
do show port-security address
```

## Ostatne

DHCP snooping

```
ip dhcp snooping
ip dhcp snooping vlan 1,10,20,100-110

int f0/2
    ip dhcp snooping trust
    ip dhcp snooping limit rate 10

ip dhcp snooping information option

do show ip dhcp snooping
do show ip dhcp snooping binding
```

Config Dynamic APR Inspection (DAI)

```
ip dhcp snooping
ip dhcp snooping vlan 1,10,20,100-110
ip arp inspection vlan 10

int g0/1
    ip dhcp snooping trust
    ip arp inspection trust

ip arp inspection validate { [src-mac] [dst-mac] [ip [allow-zeros]] }
```

Config a overenie portfast

```
spanning-tree portfast default
int f0/1 - 10
    spanning-tree portfast

int f0/20
    spanning-tree portfast disable

do show spanning-tree int f0/1 portfast
do show run | begin span
do show span summary
```

Config BPDU guard

```
span portfast bpduguard default

int f0/2
    span bpduguard enable

do show int status err-disabled
```

## Utoky

Linux commands

```shell
macof -i eth0
macof -i eth0 2>/dev/null

yesinia
```

# WLAN

Wireless LAN

|                 | PAN                               | LAN                 | MAN                                | WAN                        |
| --------------- | --------------------------------- | ------------------- | ---------------------------------- | -------------------------- |
| Standards       | Bluetooth <br>`802.15.3`          | `802.11`            | `802.11` <br>`802.16` <br>`802.20` | GSM <br>CDMA <br>Satellite |
| Speed           | <1 Mbps                           | 11 - 54 Mbps        | 10 - 100+ Mbps                     | 0.1 - 2 Mbps               |
| Range           | 6 - 9 m                           | 90 m                | Mesto                              | Narodne/Medzinarodne       |
| Applications    | Peer-to-Peer <br>Device-to-Device | Enterprise Networks | Last Mile Access                   | Mobile Data Devices        |
| Frequency range | 2.4 GHz                           | 2.4 GHz <br>% GHz   |                                    | Licencovane                |

## Zakladne pojmy

### Kodovanie

Prevod prenasanych dat do symbolov  
Zmena z jednej formy na druhu pomocou algoritmu  
Vhodnejsie na prenos, rychlejsie, detekcia chyb, samosynchronizacia, znizenie objemu (kompresia) a pod.

### Sirka Pasma (band)

Skupina frekvencii vyuzita za nejakym ucelom

- AM radiove vysielanie je 550 - 1720 MHz
- WiFi (pre 2.4 GHZ) je od 2.412 do 2.484 GHz
- WiFi (pre 5 GHZ) je od 5.150 do 5.825 GHz

### Nosny signal (Carrier signal)

Signal urcitej rekvencie, vhodnej na prenost  
Sam o sebe nema informacnu hodnotu  
Informacia sa pridava modulaciou

### Modulacia

Zmena istej charakteristiky prenasaneho signalu, ktorou bude vyjadreny prenasany symbol pocas prenosu

- Amplitudova modulacia
- Fazova modulacia
- Frekvencna modulacia

### Kanal (Channel)

Prijimac aj vysielac ocakavaju nosny signal, ale s malymi zmenami (kvoli modulacii) = kanal

### Frekvencna schema

Sposob, aky vysielac obsadzuje rozsah frekvencii v danom kanali

## Radiofrekvencny signal

- Vplyv prostredia
  - Odraz (reflection)
  - Lom (refraction)
  - Absorbcia (absorbtion)
  - Rozptyl (scattering)
  - Ohyb (diffraction)

## Manazment kanalov

| _Notice this_ | Identifier | Center frequency | Frequency range \[MHz\] | Americas | Europe, Middle East, Asia | Japan |
| ------------- | ---------- | ---------------- | ----------------------- | -------- | ------------------------- | ----- |
| ->            | **1**      | **2412**         | **2401 - 2423**         | x        | x                         | x     |
|               | 2          | 2417             | 2406 - 2428             | x        | x                         | x     |
|               | 3          | 2422             | 2411 - 2433             | x        | x                         | x     |
|               | 4          | 2427             | 2416 - 2438             | x        | x                         | x     |
|               | 5          | 2432             | 2421 - 2443             | x        | x                         | x     |
| ->            | **6**      | **2437**         | **2426 - 2448**         | x        | x                         | x     |
|               | ...        |                  |                         |          |                           |       |
| ->            | **11**     | **2462**         | **2451 - 2473**         | x        | x                         | x     |
|               | 12         | 2467             | 2456 - 2478             |          | x                         | x     |
|               | 13         | 2472             | 2461 - 2483             |          | x                         | x     |
|               | 14         | 2477             | 2466 - 2488             |          |                           | x     |

> Step = 5MHz, Range = 22MHz

Kanaly 1, 6 a 11 sa neprekryvaju  

|Identifier|Center frequency|Frequency range \[MHz\]|
|---|---|---|
|1|2412|2401 - 2423|
|6|2437|2426 - 2448|
|11|2462|2451 - 2473|

Pouzitie ktorychkolvek inych kanalov sposobuje interferenciu  
Na jednom uzemi preto mozu byt pouzite max 3 APs

### Pri 5GHz

Kanalov je 24

> Step = 20 MHz

Neprekryvajuce sa kanaly: 36, 48, 60

### Channel Reuse

![](../others/images/siete_channel_reuse.png)

Oblast je pokryta tak, aby sa jednotlive kanaly neinterferovali _(is that even a word?)_

### Channel bonding

Kombinacia dvoch 20 MHz kanalov do jedneho 40 MHz kanala

## Modulacne techniky

Ak je dopyt po konkretnom bezdrotovom kanali prilis vysoky, moze dojst k jeho nadmernemu nasyteniu (oversaturation), co by znizilo kvalitu komunikacie

- Direct Sequence Spread Spectrum (DSSS)
  - Rozprestrenie spektra priamou postupnostou
  - Pouzivane na zariadeniach `802.11b`
  - Uzitocne data, ktore sa prenasaju sa kombinuju s prudom pseudonahodnych kodov
    - Pseudonahodne kody su tzv. **chips** (8 alebo 11 chips na 1 bit)
    - Pomocou nich sa do dat prida sum, ktory sposobi rozprestrenie frekvencneho pasma (zvysenie odolnosti)
    - Je potrebna synchronizacia
- Frequency Hopping Spread Spectrum (FHSS)
  - Rozprestrenie spektra frekvencnymi skokmi
  - Rychle prepinanie nosneho signalu medzi mnohymi frekvencnymi kanalmi
  - Vysielac a prijimac musia byt synchronizovane
  - Pouziva sa v povodnom standarde `802.11`
  - Vysielac a prijimac prechadzaju medzi frekvenciami v danom kanali podla istej pseudonahonej postupnosti
    - Sekvencia obsahuje az 78 frekvencii, medzi ktorymi moze skakat
    - Vysielac medzi nimi rychlo skace (hopping)
    - Lepsie rozprestretie, vyuzitie kanala
  - V kazdom casovom momente sa vyuziva len jedna konkretna frekvencia
  - Ak prenos ramca zlyha, prenesia sa znovu na inej frekvencii
- Orthogonal Frequency Division Multiplexing (OFDM)
  - Multiplex s ortogonalnym frekvencnym delenim
  - Podmnozina multiplexovania
  - Rodina modulacnych technik, ktore vyuzivaju rozdelenie kanala na tzv. subkanaly
  - Prenos kanalmi moze bezat simultanne
  - Komplexna technika pouzivana vo vysokorychlostnych prenosoch
  - `802.11a/g/n/ac`
  - ![OFDM](../others/images/siete_ofdm.png)

## Standardy IEEE 802.11

| IEEE standard       | Year adopted | Frequency   | Max Data Rate (practical max data rate) | Max range | Note                                                            |
| ------------------- | ------------ | ----------- | --------------------------------------- | --------- | --------------------------------------------------------------- |
| `802.11a`           | 1999         | 5 GHz       | 54 Mbps (25 Mbps)                       | 400 ft    |                                                                 |
| `802.11b`           | 1999         | 2.4 GHz     | 11 Mbps (5 Mbps)                        | 450 ft    |                                                                 |
| `802.11g` (WiFi 4)  | 2003         | 2.4 GHz     | 54 Mbps (27 Mbps)                       | 450 ft    | Spatne kompatibilny s `802.11b`                                 |
| `802.11n`           | 2009         | 2.4 / 5 GHz | 600 Mbps (74 Mbps)                      | 825 ft    | Spatne kompatibilny, MIMO                                       |
| `802.11ac` (WiFi 5) | 2014         | 5 GHz       | 1 Gbps (350 Mbps)                       | 1000 ft   | Spatne kompatibilny s `802.11b/g/n`, SU MIMO, MU MIMO (CSMA/CA) |
| `802.11ac` Wave 2   | 2015         | 5 GHz       | 3.5 Gbps                                | 10 m      |                                                                 |
| `802.11ad`          | 2016         | 60 GHz      | 7 Gbps                                  | 30 ft     |                                                                 |
| `802.11af`          | 2014         | 2.4 / 5 GHz | ~26 - ~570 Mbps (depending on channel)  | 1000 m    |                                                                 |
| `802.11ah`          | 2016         | 2.4 / 5 GHz | 347 Mbps                                | 1000 m    |                                                                 |
| `802.11ax` (WiFi 6) | 2021         | 2.4 / 5 GHz | 10 Gbps (9.6 Gbps)                      | 1000 ft   | Spatne kompatibilny s `802.11b/g/n`                             |
| ...                 |              |             |                                         |           |                                                                 |

> 1 ft = ~0.3 m  
> 1 m = ~3.3 ft

### `802.11ax` (WiFi 6) upgrades

- MU-MIMO
  - Multi User Multi Input Multi Output
  - Aj pre downlink aj pre uplink
  - Max 8 kanalov _(?)_
- 1024-QAM
  - Cim vyssi QAM level, tym viac dat je mozne obsiahnut v signali = vyssia rychlost prenosu
  - 10 bits per symbol
  - O 25% vyssia ako 256-QAM pri `802.11ac` (WiFi 5) - 8 bits per symbol
- OFDMA
  - Orthogonal Frequency Division Multiplexing
  - Rozsirenie OFDM
    - 1 user = 1 kanal
  - OFDMA
    - Viacej users mozu vyuzivat nevyuzite casti kanala
- BSS coloring
  - Userov v OFDMA kanali treba odlisit
  - Individualne zariadenia sa oznackuju, nasledne aj pakety
- TWT mechanizmus
  - Target Wakeup Time
  - Aby sa viacero zariadeni nezobudzalo naraz a nesposobilo kolizie

## Komponenty WLAN

- Bezdrotovy klient
  - Koncova stanica
  - Jej konektivita je zabezpecena specializovanou bezdrotovou sietovou kartou (NIC)
  - Obsahuje radiovy vysielac a prijimac
  - Existuje v roznych vyhotoveniach s roznymi rozhraniami
    - USB, interny, PCMCIA
- Wireless Home Router
  - Integruje viac zariadeni
    - Access Point - bezdrotovy pristup
    - Ethernet switch - kablove prepojenie zariadeni
    - Router (WAN rozhranie) - default GW pre vsetkych klientov
- Pristupovy bod - Access Point (AP)
  - Typicky pripojeny metalickym mediom
  - Prepojenie LAN a WLAN
  - Klient sa potrebuje asociovat s AP pre pripojenie do siete
  - Rozne vyhotovenia (vonkajsie/vnutorne)
  - Rozne kategorie nasadenia
    - Autonomne AP
      - Kazde sa spravuje individualne cez CLI alebo GUI
      - Problem ak je vela APs
      - Je mozne spravit tzv. Klastrovanie bez podpory kontrolera
        - Viacej APs, jedna konfiguracia pre vsetky, jednotne riadenie jednotne
    - Controller-based AP
      - Wireless LAN Controller - WLC
      - Dalsia entita, ktora kontroluje a manazuje jedno alebo viac APs
      - AP je potom nazyvane LWAP - Lightweight AP
        - LWAPP - Lightweight Access Point Protocol - komunikacia medzi AP a WLC
- Bezdrotove mosty
  - Most (Bridge)
    - Zabezpecuje prepojenie 2 separatnych LAN sieti
    - Point-to-point alebo point-to-multipoint
    - Casto pouzivaju upraveny komunikacny protokol
  - Opakovac (Repeater)
    - Zvacsenie plochy pokrytej signalom
    - Znizuje sa efektivna prenosova rychlost
    - Potrebne prekrytie signalu aspon 50% - tzv. catchment area
  - Anteny
    - Rozne druhy (vsesmerove, sektorove, smerove, MIMO (viacero anten (8)))
    - Lisia sa druhom konektora, kablom, ziskovostou, smerovostou, ...
    - Cisco pouziva RP-TNC konektory

## Sposoby prevadzky WiFi

### Ad hoc mod

Prepojenie peer-to-peer bez AP(nie Bluetooth)  
Topologia sa oznacuje ako IBSS (Independent Basic Service Set)

### Tethering

Mobilny smartphone/tablet s pristupom do internetu sa pouzije ako osobny hostspot

### Mod infrastruktura

Prepojenie klientov do siete pomocout AP  
2 topologicke bloky

- BSS (Basic Service Set)
  - Pouziva AP na pripojenie klientov
  - Iba jedno AP
  - Klienti v roznych BSS medzi sebou nemozu komunikovat
- ESS (Extended Service Set)
  - Viacero BSS prepojenych tzv. Distribucnym systemom (kable, switche)
  - Viac APs

## Cinnosti WLAN

### Service Set ID - SSID - Identifikator siete

V jednom priestore moze byt dostupnych niekolko BSS alebo ESS  
Treba jednoznacne identifikovat - Service Set ID (SSID), resp. Extended SSID (ESSID) - slovny nazov bezdrotovej siete - zakladny parametrom WLAN klienta  
AP moze SSID vysielat v tzv. **Beacon ramcoch**, ale moze byt aj skryte  
Jeden AP moze navonok prezentovat niekolko SSID - kazde ma samostatnu VLAN  
AP vyuziva trunking a 802.1Q znackovanie na roztriedenie ramcov medzi SSID/VLAN

### Base Service Set ID - BSSID

V jednej ESS sa moze klient asociovat k roznym pristupovym bodom  
BSSID je identifikator konkretneho AP  
Ma formu MAC adresy

### Sposob objavenia AP klientom

#### Passive mode

AP ohlasuje sluzby posielanim broadcast ramcov nazyvanych _beacons_  
Ramce obsahuju SSID, podporovane standardy a bezpecnostne nastavenia  
Hlavnou ulohou beacons je dat vediet klientom o APs v oblasti

#### Active mode

Klient musi vediet meno SSID pretoze beacons sa nevysielaju  
Klient iniciuje proces poslanim **PROBE** poziadavky (SSID a podporovane standardy klienta) na viacerych kanaloch  
Tento postup je nevyhnutny ak AP neposiela beacons

### Komunikacia vo WLAN

Proces pristupu klienta k WLAN

1. Objav AP
2. Autentifikuj sa voci AP (open, shared key)
3. Asociuj sa s AP

Stavy klienta

- Unauthenticated, Unassociated - default stav
- Authenticated, Unassociated - klient preukazal identitu, ale nie je trvale prihlaseny k zvolenemu AP
- Authenticated, Associated - klient je prihlaseny k AP a ma plnu konektivitu

Potrebna dohoda na parametroch

- SSID
- Heslo (shared key)
- Network mode (802.11 standard)
- Security mode (WEP, WPA, WPA2, ...)
- Channel settings (frekvencne pasmo)

### CSMA/CD

Carrier Sence Multiple Access with Collision Avoidance  
Nemozeme pouzit CSMA/CD - stanica nevie ci sposobila koliziu  
Mame ale modifikaciu klasickej metody - vyhnutie sa kolizii - CA

Prijimajuci host posiela ACK kratko po prijati spravy  
Ak ACK nie je prijate - prenost sa uskutocnuje znovu

#### Distribuovana koordinacna funkcia (DCF)

1. User A pocuva, a zisti, ze nikto neprenasa, prenesie ramec, zaroven da info o dobe trvania prenosu
2. User B ma ramec, ale musi pockat kym skonci prenos A + kym uplynie DIFS cas
3. B pocka nahodny backoff cas, kym sa pokusi znova preniest frame

#### RTS/CTS

Request to sender

- Dohladovy ramec, v ktorom stanica informuje prijemcu, ze mu chce poslat data
- Informuje o potrebnom case na tento prenos

Clear to Send

- Dohladovy ramec, v ktorom prijemca potvrdzuje prijem ziadosti RTS
- Informuje o potrebnom zvysnom case na tento proces

Vymena instruuje vsetkych v dosahu dodrzat ticho a nekomunikovat

## CAPWAP protokol

Umoznuje WLC manazovat viacere APs  
Zalozeny na LWAPP  
Pridava bezpecnost s DTLS (Datagram Transport Layer Security)  
Zapuzdruje a preposiela WLAN prevadzku medzi AP a WLC cez tunel (UDP 5246, UDP 5247)  
Podporuje aj IPv4 (typ 17) aj IPv6 (typ 136)

### Split MAC architektura

Robi vsetky funkcie ktore bezne vykonavaju APs a distribuuje ich medzi funkcne komponenty

| AP MAC functions                         | WLC MAC functions                                  |
| ---------------------------------------- | -------------------------------------------------- |
| Beacons and probe responses              | Authentification                                   |
| Packet ack and retransmission            | Association and reassociation of roadming clients  |
| Frame queueing and packet prioritization | Frame translation to other protocols               |
| MAC layer data encryption and decryption | Termination of 802.11 traffic on a wired interface |

### DTLS sifrovanie

Poskytuje bezpecnost medzi AP a WLC  
By default zabezpecenie kanala zapnute, sifrovanie dat vypnute

### Flex Connect APs

Pre konfiguraciu a riadenie APs cez WAN linku

Existuje v 2 modoch

- Connected mod
  - WLC je dostupne
  - AP ma konektivitu cez CAPWAP tunel
  - WLC vykonava vsetky CAPWAP funkcie
- Standalone mod
  - WLC je nedostupne
  - AP stratilo CAPWAP konektivitu s WLC
  - AP moze prevziat niektore z funkcii WLC na seba lokalne (prepinanie datovej prevadzky klinetov, autentifikacia klientov)

## Planovanie WLAN nasadenia

Co treba brat do uvahy

- Pocet pouzivatelov
- Planovana priepustnost
- Ake rychlosti pouzivatelia ocakavaju
- Pouzitie neprekryvajucich sa kanalov APs
- Nastavenie vysielajuceho vykonu

Co treba zvazit

- Napojenie na kabelaz
- Napajanie
- Ci je priestor na umiestnenie
- Umiestnenie AP nad prekazky
- Umiestnenie tesne pod strop v kazdej oblasti
- Umiestnit AP tam, kde budu pouzivatelia

## WLAN hrozby

- WLAN je otvorena pre vsetkych v dosahu (s prislusnymi povereniami (credentials))
- Utoky - umyselne, neumyselne, zvonku, z vnutra
- Osobitna nachylnost na
  - Odpocuvanie udajov
  - Bezdrotovych votrelcov
  - DoS utoky - pri nespravne nakonfigurovanych zariadeniach
  - Podvrhnute APs (Rogue APs) - zhromazdovanie udajov o klientoch (MAC, pakety), pristup k sietovym zdrojom, MITM attack - ochrana - monitorovaci software
  - Nahodne rusenie
    - Mikrovlnky, bezdrotove telefony, detske vysielacky, ...
    - 2.4 GHz je viac nachylne ako 5 GHz

### Managemet Frame DoS Attacs

- Utok falosnym odpojenim (Spoofed Disconnect Attack)
  - Utocnik posle sekvenciu prikazov pre diasociaciu (odpojenie) vsetkym bezdrotovym klientom
  - Klienti sa znova pokusia pripojit = zahltenie
- Zaplava CTS ramcami (CTS flood)
  - Utocnik pokryje celu sirku pasma
  - Opakovane zaplavuje siet ramcami CTS na stanicu ktora neexistuje
  - Vsetci klienti prijimaju CTS a musia cakat kym utocnik neprestane vysielat ramce

## Zabezpecenie WLAN

Maskovanie SSID (SSID cloaking) - zakazat posielanie SSID v beacon ramcoch - klientov treba manualne nakonfigurovat

MAC address filtering - manualne povolit/zakazat pristup na zaklade MAC

### Autentifikacia pouzivatelov

- Open system - nic sa nedeje, klient poziada o pristup a dostane ho, idealne pouzit VPN
- Shared key - heslo - WEP, WPA, WPA2 (`802.11i`), WPA3 (utocnik moze odchytit autentifikacny dialog a dokaze ziskat heslo (riesene pomocou EAP alebo WPA2))

EAP

- Protokol urceny na prenasanie roznych autentifikacnych dialogov medzi klientom (supplicat) a bodom vyzadujucim autentifikaciu (authenticator)
- Metody nad EAP
  - LEAP (Lightweight EAP) - Cisco, overenie pomocou mena a hesla
  - PEAP (Protected EAP) - dvojfazova overovacia schema
    1. Pomocou TLS sa vybuduje bezpecne sifrovane spojenie a overi sa autenticita servera (TLS certifikat)
    2. Volitelnym dalsim sposobom sa overi autenticita klienta
  - EAP-TLS (EAP - Transport Layer Security)
    - Vzajomne overenie klienta aj servera
    - Vybuduje sa bezpecne spojenie
    - Vyzaduje si certifikaty aj pre klienta aj pre servera
  - Vela dalsich metod
  - Pre multi-vedor environment je vhodne PEAP alebo EAP-TLS

### Autentifikacia siete

Tak ako klientov, aj siet treba autentifikovat (Rogue AP)  
Tu su tiez vhodne metody kde sa server preukazuje certifikatom (PEAP, EAP-TLS, EAP-TTLS,...)

### Zabezpecenie prenasanych dat

Pasivne odpocuvanie nie je mozne detegovat  
Radiovy signal nie je mozne lahko ohranicit  
Treba akceptovat ze prevadzka bude odpocuvana a zamerat sa na to, aby odpocuvanim utocnik nic neziskal  
Vhodne je sifrovanie prenasanych dat  
`802.11b/g` obsahuje klasicku implementaciu sifrovania obsahu - WEP (Wired Equivalent Privacy)

WEP

- Dnes uz velmi slabe
- Staticky kluc
- Kluc je identicky s klucom pre volitelnu autentifikaciu
- Symetricka sifra
- Standardne 64-bit kluc (40+24), neskor 104 alebo 128-bit

WPA

- Nahrada za WEP
- 128 + 48 bit
- Kluc je priebezne aktualizovany TKIP protokolom (Temporary Key Integrity Protocol)
- Kazdy ramec je sifrovany inym klucom (odvodenym od zakladneho kluca)

WPA2

- Vyuziva sifrovaci algoritmus AES (Advanced Encryption Standard)
- Namiesto TKIP pouziva CCMP (Counter Cipher Mode with Block Chaining Message Authentication Code Protocol)

### Autentifikacia v organizacii

Enterprise security mode - vyzaduje AAA server (Authentication, Authorization, Accounting) RADIUS server  
Potrebne definovat 3 informacie

- RADIUS server IP address
- UDP ports
  - Vacsinou 1812 RADIUS Authentication, 1813 RADIUS Accounting
- Shared Key
