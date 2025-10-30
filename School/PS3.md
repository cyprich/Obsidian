# PS3

Pocitacove Siete 3

## Uvod

Predmet zamerany na reatime multimedialnu (najma hlasovu) komunikaciu nad uz existujucou sietou  
Konvergovana siet  
"Close to realtime"  
Protokol **SIP**  
Komunikator - program (?) ktory umoznuje komunikaciu  
RTT max 250 ms

## DNS a ENUM

### Domain Name System (DNS)

_"Nieco na styl telefonneho zoznamu pre internet"_  
Ludia si lahsie pamataju slovne nazvy nez IP adresy

Spociatku sa preklad riesil rucne vytvaranymi databazami - subor `HOSTS.TXT` kedysi dostupny cez FTP na adrese `26.0.0.73`

RFC 1034 and 1035 a mnohe dalsie

Port 53

- TCP ak sprava >512 B, alebo sa prenasaju cele zony
- UDP v ostatnych pripadoch (velkost <=512 B zarucuje, ze paket nemusi byt fragmentovany ani pri najmensom MTU - 576B)

Takmer zadadne sa pouziva UDP (nizka rezia)

Standardne nesifrovany, ale su riesenia

- DNS over TLS (DoT)
  - RFC 7858, 8310
  - TCP 853
  - Podporovane servery - `1.1.1.1`, `9.9.9.9`
- DNS over HTTPS
  - RFC 8484

#### Pojmy v DNS

**Domena**

- Hierarchicka struktura nazvov objektov, ktora vyjadruje ich prislusnost do istej oblasti, napr:
  - Krajina, stat, vlastnik, interna struktura vlastnika
  - Napr. `sk` - `uniza.sk` - `fri.uniza.sk` - `kis.fri.uniza.sk`
- Uplne "hore" je `.`, cize spravne by adresa mala byt napr. `uniza.sk.`, ale nie je potrebne to pisat

**Name servers** - Menne servery, Nazvove servery

- Servery udrzuju databazu mien a zdrojovych zaznamov - **resource records**
- Klient
- Server zodpoveda za zonu
- Server ktory spravuje zonu sa nazyva **autoritativny**

**Resolver**

- Klientska aplikacia, ktora ziskava informacie z DNS serverov
- Stub - hlupy - uplne spolieha na server a ocakava definitivnu odpoved
- Rekurzivny - nehlupy - spracuje aj ciastocnu odpoved, ak ho server "odkaze" na iny server, vie si s tym poradit

Kazdy uzol ma svoje vlastne meno (hostname) a patri do istej domeny

- hostname + domena = **Fully Qualified Domain Name** (FQDN)
- Komponenty FQDN sa oddeluju bodkou a volaju sa **labels** - max 63 znakov dlhy, `[a-z][A-Z][0-9]`, standardne len ASCII znaky, da sa aj inak
- FQDN celkovo max 255 znakov

**Resource record** - Zdrojovy zaznam

- Prvok databazy v DNS
- Casti
  - Vlastnik
  - Typ - druh informacie
  - Trieda - rodina protokolov
  - TTL - doba platnosti
  - RDATA - informacne telo
- Typy
  - SOA - Start of Authority - popis zony, povinny, musi byt prave raz - domenove meno, identita spravcu, seriove cislo, casoe udaje pre sync
  - A - IPv4
  - AAAA - IPv6
  - NS - Name Server - FQDN autoritativneho DNS servera danej zony
  - CNAME - canonical name - alias
  - MX - Main Exchanger - postovy server
  - PTR - Pointer - reverzny lookup - IP na FQDN
  - SRV - Service - lokalizacia servera
  - NAPTR - Naming Authority Pointer - vyuzivane pri ENUM, preklad telefonneho cisla do ineho formatu
- Trieda
  - IN - Internet - jedina pouzivana dnes
  - CH - Chaosnet - experimentalna siet na MIT
  - HS - Hesiod - tiez uz nepouzivane

**Zona**

- Podstrom domenoveho stromu
- Ako root celeho stormu je `.` (bodka)
- Zony mozu byt delegovane na nove autoritativne servery - zverim spravovanie niekomu inemu
  - Napr. `sk` oddeli `telecom.sk` nech si spravuju sami, `uniza.sk`, `edu.sk`, ...

#### Princip prace DNS

Vsetky domeny su organizovane v stromovej strukture  
Koren stromu je `.`  
Pre domenu `.` existuje 13 autoritativnych serverov - `A.ROOT-SERVERS.NET`, `B.ROOT-SERVERS.NET`, ..., `M.ROOT-SERVERS.NET`  
Tieto stromu obsahuju zoznam vrcholovy domen - **Top-Level Domains** = TLD  
Autoritativne servery TLD obsahuju zoznam domen druhej urovne, ich prislusne autoritativne servery a delegovania  
Takto sa ide rekurzivne do hlbky  
Ked klient potrebuje hladat v DNS, kontaktuje svoj prednastaveny DNS server  
Ak tento server nepozna "odpoved", tak bud ju rekurzivne hlada u svojho parenta, alebo odkaze na parenta (podla toho co pozadoval klient)

DNS sprava ma fixnu strukturu

- Header
- Question
- Answer
- Authority - info o DNS serveri, ktory ma presne alebo presnejsie info o hladanom mene
- Additional - pridavne pomocne informacie

#### Vytvorenie vlastnej zony

Musi obsahovat prinajmensom

- SOA zaznam - vlastnik, meno servera z ktoreho informacia povodne pochadza, administrativny kontakt, seriove cislo zony, casy refresh, retry, expire, negative TTL
- NS zaznam - odkaz na mena autoritativnych serverov - nema byt IP ani CNAME

Pri kazdej aktualizacii zony sa seriove cislo musi inkrementovat - idealne `YYYYMMDDrr`, kde `rr` je cislo revizie

Problem - ak je autoritativny NS je vo vnutri svojej domeny

- Napr. domena `gvoza.sk`, NS `ns.gvoza.sk`
- Riesenie - pridanie tzv. _glue record_ - `A` alebo `AAAA` zaznam, ktory nazvu z NS zaznamu v tej istej delegujucej zone prideluje IP adresu - inak by sa glue nemal pouzivat
  - Cize basically ze dame rovno IP adresu NS, nie jeho meno, cim sa porusi to co sa pisalo predchvilou

```txt
example.com.  NS  ns1.example.com
example.com.  NS  ns2.example.com
ns1.example.com   A   123.123.123.123  // toto je glue record
ns2.example.com   A   124.124.124.124  // toto je glue record
```

IPv4 a IPv6 su pre DNS len protokolmi na prenos jeho sprav, ale nemaju vplyv na to, co prekladaju

- Kontaktujeme server po IPv4 ale moze nam vratit IPv6 odpoved
- Kontaktujeme server po IPv6 ale moze nam vratit IPv4 odpoved

#### DNS v SIP - zaznam SRV

Vyuzivaju sa na lokaciu SIP proxy servera zodpovedneho za danu domenu

#### Vsuvka o regex

Budeme sa venovat tzv. POSIX-compatible regular expressions

| Znak                | Popis                                                      | Priklad                                                                                                           | Antipriklad                |
| ------------------- | ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | -------------------------- |
| `.`                 | Jeden lubovolny znak                                       | `a.c` = `abc`, `acc`, `adc`, ...                                                                                  | `a.c` != `abbc`, `abcc`    |
| `^`                 | Zaciatok riadku                                            | `^abc` = `abcdef`, `abc1234`                                                                                      | `^abc` != `123abc`, `aabc` |
| `&`                 | Koniec riadku                                              | `abc&` = `abcabc`, `123abc`                                                                                       | `abc&` != `abcd`           |
| `[]`                | Jeden z mnoziny znakov                                     | `a[123]b` = `a1b`, `a2b`, `a3b`; `a[x-z]b` = `axb`, `ayb`, `azb`; `a[pq1-3]b` = `apb`, `aqb`, `a1b`, `a2b`, `a3b` |                            |
| `()`                | Podkupina/blok, neskor s odkazom `\n`, kde $n \in {1 - 9}$ | `(abra)kad\1` = `abrakadabra`; `(wifi) je \1, ale (kabel) je \1` = `wifi je wifi, ale kabel je kabel`             |                            |
| `*`                 | Predchadzajuci znak sa nachadza 0 alebo viac krat          | `ab*c` = `ac`, `abc`, `abbc`, `abbbc`, ...                                                                        | `ab*c` != `acc`            |
| `+` (iba v ERE)     | Predchadzajuci znak sa nachadza 1 alebo viac krat          | `ab+c` = `abc`, `abbc`, `abbbc`, ...                                                                              | `ab+c` != `ac`             |
| `?` (iba v ERE)     | Predchadzajuci znak sa nachadza 0 alebo 1 krat             | `ab?c` = `ac`, `abc`                                                                                              | `ab?c` != `abbc`           |
| `{n}` (iba v ERE)   | Predchadzajuci znak sa nachadza presne `n` krat            | `a{3}` = `aaa`                                                                                                    |                            |
| `{n,}` (iba v ERE)  | Predchadzajuci znak sa nachadza aspon `n` krat             | `a{3,}` = `aaa`, `aaaa`, `aaaaa`, ...                                                                             |                            |
| `{n,m}` (iba v ERE) | Predchadzajuci znak sa nachadza medzi `n` a `m` krat       | `a{4,5}` = `aaaa`, `aaaaa`, ...                                                                                   |                            |

Zameny

- Nieco ako _find and replace_
- Pouziva sa delimiter - casto `!` a vyzera takto: `!hladanyvyraz!nahrada!`
- Napr.
  - `!abc!XYZ!` - ak najde `abc`, tak ho nahradi `XYZ`
  - `!mailto:!!` - ak najde `mailto:` tak ho nahradi za nic = da ho prec
  - `!^(43[0-5][0-9])$!sip:\1@uniza.sk!` - ak je cislo z rozsahu 4300 az 4359, tak ho nahradi za `sip:43..@uniza.sk` (SIP URI)

### ENUM

ITU-T E.164

Oznacenie pre cislo nielen verejnej telefonnej siete

Podla odporucania max. 15 znakov

- Kod krajiny = 1-3 cislice
- Geograficka oblast = $x$ cislic
- Cislo ucastnika = $15 - x$ cislic

Ak sa cislo zacina `0` - narodny hovor - `0902 ...`
Ak sa cislo zacina `00` alebo `+` - medzinarodny hovor - `+421 902 ...`

Napr. `421 41 513 4301`, `421 905 123 456`

Cislo v E.164 je mozne vyhladat v DNS

1. Zoberieme cislo - `+421 41 513 4301`
2. Odstranime neciselne znaky - `421415134301`
3. Medzi kazdu cislicu vlozime bodku - `4.2.1.4.1.5.1.3.4.3.0.1`
4. Medzi kazdu cislicu vlozime bodku - `1.0.3.4.3.1.5.1.4.1.2.4`
5. Pridame retazec `e164.arpa` na koniec - `1.0.3.4.3.1.5.1.4.1.2.4.e164.arpa`

Ak ma IP telefonia nahradit normalnu, musi byt aspon taka kvalitna  
Okrem dalsich zalezitosti treba mat mechanizmus na preklad `E.164` cisla na `SIP`/`H.323`/...  
Toto sa nazyva _E.164 Numbering Mapping_ = **ENUM**

## Session Initiation Protocol (SIP) a suvisiace protokoly

RFC 2543  
RFC 3261  
Velmi vela rozsirujucich RFC

IP telefonia, telekomunikacia, multimedialna komunikacia  
Klucovy protokol pre

SIP je stabilna a odskusana technologia s mnohymi produktmi a rieseniami  
Funguje na mnohych otvorenych komercnych produktoch (Cisco, ...) ale aj open-source

Vyhody - nizsie ceny, kvalitnejsie sluzby, kvalitnejsi zvuk/obraz, ...  
Nevyhody - podvody s fake ID, robocalling, telemarketing nevyziadane volania, ...

Textore a binarne protokoly

|         | Vyhody                                                                                     | Nevyhody                                                                                     |
| ------- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| Textove | Lahka citatelnost, zrozumitelnost, jednoduche ladenie, univerzalonst klientov              | Nizka efektivnost vzhladom na objem dat, nemoznost prenasat binarne data v nativnost formate |
| Binarne | Vyssia efektivita prenosu dat, kompaktny format, prirodzene vhodne na vymenu binarnych dat | Bez specialnych analyzatorov prakticky necitatelny, pri ladeni potrebni dedikovani klienti   |

### IP telefonia

Pre internet to bola "nova" sluzba koncom milenia  
Prenos hlasu (a ineho typu multimedialnych dat) cez siet v realnom case medzi 2+ ucastnikmi s pouzitim IP protokolu s pouzitim IP protokolu

### SIP signalizacia

Zostavenie spojenia pred hovorom - zvonenie

Casti?

- Lokalizacia uzivatela a mobilita
- Riesenie dostupnosti uzivatela
- Dojednavanie sposobilosti uzivatela
- Zlozenie spojenia
- Manazment nad spojenim

### Princip cinnosti

1.
2.
3.
4.
5.
6.
7.
8.
9.

### SIP Entity

- User Agent (UA)
  - Bud UA Client alebo UA Server
  - Stavovy
- SIP Server
  - Registrar
  - Proxy server
  - Redirect

Klient - entita, ktora vysiela dotazy a prijima odppovede - UAC, Proxy Server  
Server - entita, tkora prijima dotazy, spracuje ich a vysiela spat odpovede - UAS, Registrar, Redirect, Proxy Server

SIP Gateway

- Specialny typ UA
- Rozhranie medzi SIP sietou a sietou s inym signalizacnym protokolom (H.323, ISDN)
- Gateway != SIP Proxy

Back 2 Back User Agent - B2BUA

- Specialny typ UA, ktory moze modifikovat SIP spravy
- Rozbija komunikaciu na 2 polovice - Call Legs
- Volajucemu sa predstavi ako volany, volanemu ako volajuci
- Zvycajne sluzi ako ALG gateway,
- Byva sucast Session Border Controller-a

Mobilita pouzivatela

- Problem pri kontakte pomocou IP (DHCP)
- Registracia pouzivatelov

**SIP Proxy = SIP router**

Outbound Proxy - vsetky klientske spravy su najprv poslane sem

SIP Transakcia

- Ziadost a jej odpovede az po

Typy SIP Proxy serverov

- Stateless
- Statefull

## SDP

Session Description Protocol

Informacie poskytovane v SDP

- Popis a poziadavky spojenia (session description) - popis relacie a ucel, doplnkove info
- Caseove informacie (timing informations) - cas zaciatku a konca, cas opakovania
- Info o tom, ci je relacia privatna alebo verejna
- **Popis medii a transportne informacie**
- Dalsie info o spojeni
- Jazyk

## Transportne protokoly vo VoIP aplikaciach

Transportna vrstva - TCP, UDP

- Segmentacia rekombinacia dat
- Dorucovanie medzi aplikaciami
- Spolahlivost
- Spojovanost
- Virtualne okruhy
- Riadenie toku

### Transportne protokoly pre media

Real-time data

- Generovane v realnom case
- Vysoke naroky na oneskorenie a jitter
- Segmenty vyzaduju dorucenie do isteho momentu, potom su nepouzitelne
- Relativne necitlive na straty

Nehodi sa ani TCP ani UDP

- TCP - velka rezia,
- UDP - nespojovane,

#### UDP-Lite

RFC 3828

UDP ma checksum ktory chrani cely segment - ak je `0x0000` tak sa nema overovat  
Zahadzuje nespravne pakety alebo akceptuje vsetky

UDP-Lite  
Pole `length` je nahradene `checksum coverage`  
Pocet bajtov ktore su chranene checksumom

Treba pamatat ze L2 technologie verifikuju cely ramec, ak je poskodeny tak sa zahodi cely

#### Datagram Congestion Control Protocol (DCCP)

RFC 4340

Vychadza z dobrych vlastnosti TCP a UDP a pridava vlastne

- Riesenie zahltenia (neriesi flow control)
- Spojovanost - Request, Response, Ack; CloseReq, Close, Reset
- Nespolahlivost
- +Informuje o oneskori a stratach paketov

Sekvencne ocislovane

Data sa prenasaju v segmentoch typu `Data`

Hlavicka

- 12 bajtov pre 24-bit sekvencne cisla
- 16 bajtov pre 48-bit sekvencne cisla

Vyssi overhead oproti UDP

#### Real-time Transport Protocol (RTP)

RFC 3550

Pouziva existujuce transportne protokoly (tuto konkretne UDP)  
Riesi veci na aplikacnej vrstve  
Vzdy implementovany ako _userspace_ kniznica

Realne sa v praxi pouziva

Vhodny pre unicast aj multicast

Cislovanie paketov  
Casova synchronizacia  
Identifikacia typu obsahu  
Neposkytuje riadenie toku

Mixer pri CSRC (Sending & Contributing Source)

Pouziva sa pri

- IP telefonii
- Videokonferencie
- Streming dat (Sanet TV)

Je nezavisly od signalizacie - SIP, H.323, SCCP, H.248

##### RTCP - RTP Control Protocol

Podporny protokol pre RTP

#### Prehlad hlasovych kodekov

Kodek = koder-dekoder  
Prevod hlasu medzi analogovou a digitalnou formou

Zavisi od neho

- Kvalita preneseneho hlasu
- Tolerancia na straty
- Vnesene oneskorenie
- Naroky na prenosove pasmo

Kvalita kodekov sa uvadza v tzv. **MOS** mierke - Mean Opinion Score

Najbeznejsia digitalizacia hlasu prebieha v 3 krokoch

- Vzorkovanie - rozseknutie na useky
- Kvantovanie - ohodnotit vysku useku
- Kodovanie - ked vieme v akom case aka vyska, tak zakodujeme

Kvrantovanie sa bezne robi v logaritmickej mierke  
Dve mierky

- $\mu$ zakon - USA, Kanada, Japonsko
- A zakon - vsetko ostatne

Kodeky v odporucaniach ITU-T

- G.711 - zakladne PCM - najcastejsie pouzivany
- G.726 - adaptivne diferencialne PCM
- G.729 - prediktivny, oblubeny, patentovo chraneny
- ...

Mimo ITU-T

- AMR
- iLBC
- CELT
- FLAC
- SILK
- ...

Doplnkove vlastnosti kodekov

- Voice Activity Detection (VAD)
  - Aby sa neprenasalo ticho ked pouzivatel nerozprava
  - Ak nereaguje dostatocne rychlo - clipping
- Comfort Noise Generation (CNG)
  - Generovanie sumu ked pocujeme ticho, aby ucastnik nemal pocit ze sa nieco pokazilo
- Packet Loss Concealment (PLC)
  - Subor roznych technik
  - Znazi sa zakryt stratu paketu

Tabulka MOS Score

| Hodnota MOS | Kvalita   |
| ----------- | --------- |
| 5           | Excellent |
| 4           | Good      |
| 3           | Fair      |
| 2           | Poor      |
| 1           | Bad       |

> Ludia mali hodnotit

Az take velke rozdiely tam nie su

### Transportne protokoly pre signalizaciu

#### Stream Control Transmission Protocol (SCTP)

RC 4960

Signalizacia - potrebujeme dat vediet druhej strane ze potrebujem spravit hovor  
Problem v paketovych sietach - je best-effort - moze sa hocico stratit

SCTP sa pokusa toto riesit

- Spojovany
- Spravovo orientovany (nie stream)
- Spolahlivy a potvrdzovany
- Riadenie toku dat
- Multistreaming
- Multihoming

Pojmy

- Asociacia - vyraz pre vytvorene spojenie
- Stream - postupnost sprav
- Chunk - nedelitelna atomicka cast jedneho SCTP segmentu, nikdy nefragmentovana

Multistreaming

- V ramci jednej asociacie (spojenia) mozeme prenasat viacero nezavislych streamov/veci/aplikacii
- Ak sa nieco strati, tak to neovplyvni ostatne streamy
- Mensi overhead oproti TCP? (vytvorenie spojenia a pod.)

Multihoming

- Dostupnost SCTP terminalu pod viacerymi IP adresami
- Zabezpecenie redundancie a failoveru
- V TCP/UDP sa neda spravit nieco podobne
- Podpora pre IPv4 a IPv6, aj sucasne

Pouzitie

- Prenos signalizacie - MTP2, MTP3, SIP
- Reliable Server Pooling
- DIAMETER - AAA

## Prechod SIP cez NAT a firewall

NAT

- Tradicke NAT
  - Basic NAT
    - Jednosmerne (outbound) - private to public
    - Jedna private IP na jednu verejnu
  - NAPT (Network )
    - Viacere privatne na jednu verejnu
    - Pouziva sa aj cislo portu
- Bi-directinal NAT (Two-way NAT)
  - Obojsmerne
  - Staticky
  - V `bind9` su to tzv. _views_
- Twice NAT
  - Ked napr. potrebujeme spojit 2 rovnake siete (`192.168.1.0/24`), napr. pri VPN
  - Preklada sa aj zdrojova aj cielova IP
  - Vymyslia sa nove siete, pre obidve siete napr. `10.0.10.0/24`, `10.0.20.0/24`
- Multihomed NAT

STUN - **S**imple **T**raversal of **U**ser Datagram Protocol Through **N**etowrk Address Translators  
Simple Traversal of UDP through NATs

Rozlisuju sa 4 zakladne implementacie NAT (RFC 3489)

- Full cone NAT
- Address restricted cone NAT
- Port restricted cone NAT
- Symmetric NAT

Vyuzivanie NAT mapovani

- Mappng Behavior
  - Endpoint-independent Mapping
  - Address-Dependent Mapping
  - Port-Dependent Mapping
- Filtering Behavior
  - Endpoint-Independent Filtering
  - Address-Dependent Filtering
  - Address and Port-Dependent Filtering

### SIP cez NAT

SIP svojim dizajnom porusuje odporucania aplikacnych protokolov - _v hlavicke nepouzivat IP adresy_ - prave kvoli NAT, ktory IPcku zmeni

Prolem SIP cez NAT/Firewall

- Prvy problem - spojenia z privatnej siete von
  - Cielova _privatna_ IP je v hlavicke L7, co sa neprelozi NATkom, a privatne adresy sa neprenasaju verejnym priestorom
  - Ciastkove riesenie - lichobeznikovy SIP Proxy
    - Ak Proxy zisti roziel medzi `via` v hlavicke a IP adresou, tak nastavi parameter `Received`
    -
- Druhy problem - spojenia na hosta v privatnej sieti
  - NAT binding sa udrzuje len nejaku dobu, ak je dlho ticho tak sa zrusi
  - Riesenie - nejaky keep alive packet

### STUN - Session Traversal Utilities for NAT

Klient-server

Klinet

- Posiela
- Binding Request? BReq

Server

- Posiela
- Binding Response? BResp

STUN testy

1. NAT Discovery
2. NAT Discovery - Full Cone
3. NAT Discovery - Symmetric NAT
4. NAT Discovery - Restricted Cone NAT

### TURN - Traversal Using Relay around NAT

RFC 5766

Relay sluzba - nieco ako Proxy v HTTP

Priklad cinnosti

1.
2.
3.
4.
5.

Malo by to byt 100% riesenie na SIP NAT traversal

Kde sa da pouzit STUN, treba pouzit STUN  
Ak STUN nefunguje, pozit TURN

### ICE - Interactive Connectivity Establishment

RFC 8839

Spaja STUN a TURN do jedneho riesenia  
Pracuje so vsetkymi typmi NAT

Host Candidate  
Server Reflexive Candidates  
Relayed Candidates

ICE call flow

1.
2.
3.
4.
5.
6.
7.
8.

### Riesenie na strane siete/poskytovatela

RTP media relay

- V podstate RTP Proxy
- Postrednik pre RTP/RTCP a UDP prudy dat

Back 2 Back User Agent (B2BUA)

- V podstate man-in-the-middle

ALG

- Application Level Gateway
- Firewall s inspekciou az do L7
- Byva sucastou FW rieseni - Cisco, Fortinet, ...

UPnP

- [www.upnp.org](www.upnp.org)
- Zariadenie si vie otvorit dieru vo FW na jednoduchu konfiguraciu
- Zahrnute v DLNA (Digital L N Alliance)
