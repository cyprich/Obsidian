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

## Bezpecnost

### Zakladne pojmy

Athentication - zistenie identity (kto)  
Authorization - zistenie pravomoci (co moze robit)  
Confidentiality, Privacy - informacie len pre opravnene osoby  
Integrity - spravnost a uplnost informacii  
Availability - informacia dostupna v momente vyziadania  
Non repudiation (nepopieratelnost povodu) - nepopretie autorstva/prijatia  
Anti-replay - ochrana pred podvrhnutim dat

### HTTP Digest autentifikacia

RFC 7235  
Autentifikacny mechanizmus zalozeny na HTTP autentifikacii  
Nasledovnik HTTP basic autentifikacie  
Siroko pouzivany roznymi textovymi protokolmi

Autentifikacia moze byt pouzita pri

- Registracii
- Vytvarani spojenia
- Zmene spojenia
- Ukoncenie spojenia

Pouziva spravy

- 401 Unauthorized
- 407 Proxy Authentication Required
- 403 Forbidden

Princip

- Pri vyzadovanej autentifikacii SIP server ziada o poskytnutie udajov
- V odpovedi 401 alebo 407 SIP server poskytuje v hlavicke parametre na vypocet has funkcie
- Klient musi zahrnut tieto hlavicky v odpovedi
  - v hlavicke Authorization + poskytnut udaje na autentifikaciu, vypocitanu na zaklade hash funkcie
  - Heslo sa vobec neprenasa

Parametre autentifikacie

- realm - domena
- qop - quality of protection - hovori ci je len autentifikacia (auth) alebo aj integrita (auth-int)
- nonce - nahodny retazec generovany serverom
- cnonce - nahodny retazec generovany klientom
- digestURI - `sip:domain` napr. `sip:kis.fri.uniza.sk`
- nonceCount - poradie nonce

Vypocet odpovede

- Hash funkcia - MD5
- Ak nie je qop definovane
  - `response=MD%(HA1:nonce:HA2)`
  - `HA1=MD5(username:realm:password)`
  - `HA2=MD5(method:digestURI)`
- Ak je qop definovane
  - `reponse=MD5(HA1:nonce:nonceCount:cnonce:qop:HA2)`
  - `HA1=MD5(MD5(username:realm:password):nonce:cnonce)`
  - `HA2=(MD5(method:digestURI:MD5(entityBody)))`

Vyhody

- Heslo nie je plaintext
- Ochrana voci podvrhnutiu relacie
- Server si pamata nonces

Nevyhody

- Man-in-the-middle utoky
- MD5 je prelomitelne

### Zabezpecenie medii

#### RTP

#### SRTP

RFC 3711

Ponuka

- Sifrovanie
- Autentifikaciu
- Integritu
- Nepopieratelnost povodu
- Ochrana pred replay utokom

Pre sifrovanie pouziva AES

K overeniu spravy a ochrane identity HMAC-SHA1

- RFC 2104
- 160bit tag z hlavicky a payloadu paketu
- Pripojeny ku sprave

Odvocenie kluca

- Hlavny kluc
- Periodicky odvodzovane kluce

Spolieha sa na externu spravu klucov

- MIKEY - Multimedia Internet Keying
- ZRTP

#### ZRTP

Zimmerman RTP  
RFC 6189  
Pouziva SRTP

Nesifruje data  
Poziva Diffie-Hellman vymenu klucov

#### RTPC

RTP Control Protocol  
RFC 3611

Neprenasa data  
Posiela statistiky (QoS, straty, round trip time)

Ak RTP bezi na porte $n$, (kde $n mod 2 = 0$), tak RTCP bezi na porte $n+1$

Asymetricke sifrovanie

- Sukromy+verejny kluc
- Ak sifrujem jednum klucom, desifrujem druhym

Sifrovanie

- Sifrujem verejnym klucom
- Desifruje len vlastnik sukromneho kluca

Elektronicky podpis

- Sifrujem sukromnym klucom
- Overi kazdy prisluchajucim verejnym klucom
- Funguje takto
  - Vytvorim Hash z dokumentu
  - Zasiftujem Has svojim sukromnym klucom
  - Pripojim zasifrovany Hash k dokumentu
- Overuje sa takto
  - Ziskam podpisany dokument
  - Ziskam verejny kluc odosielatela
  - Overim verejny kluc odosielatela - Certificate Revocation List (CRL)
  - Vypocitam Hash z dokumentu
  - Desifrujem ziskanym klucom prilozeny Hash
  - Porovnam Hash hodnoty

##### Pretty Good Privacy

Autor Philip Zimmerman

Nepouziva hierarchicky model

- Pouzivatelia si podpisuju certifikaty navzajom
- Jeden PGP certifikat - bezne viacero podpisov

Kazy PGP pouzivatel ma zoznam verejnych klucov (Keyring), ktory si moze vymienat

Pri pridavani kluca do keyringu

- Absolutna dovera
- Ciastocna dovera
- Nedovera

#### SRTPC

### S/MIME

Multipurpose Internet Mail Extension

RFC 2045-9  
RFC 4288-9

Podporuje

- Ine znakove sady ako ASCII (aj v hlavicke)
- Netextove prilohy
- Telo spravy pozostavajuce z viacerych casti

Zavadza nove hlavicky  
Ciel - nepozmenit standardy

Pouzite v SMTP, HTTP, SIP, JPEG, GIF

Priklad

```txt
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary=hranica

This is a message with multiple parts in MIME format
--hranica
Content-Type: text/plain

This is a body of the message
--hranica
Content-Type: application/octet-stream
Content-Transfer-Encoding: base64

PGh0bWw+CiAgPGhlYWQ+CiAgPC9oZWFkPgogIDxib2R5PgogICAgPHA+VGhpcyBpcyB0aGUgYm9ke
SBvZiB0aGUgbWVzc2FnZS48L3A+CiAgPC9ib2R5Pgo8L2h0bWw+Cg==
--hranica--
```

#### Zabezpecenie MIME

Ziskam certifikat druhej strany  
Zasifrujem celu MIME spravu a odoslem

Problem v SIP - halvicky musia ostat nad MIME spravou (via, from, to, call-uid, cseq)

#### S/MIME

Secure/Multipurpose Internet Mail Extensions  
RFC 5751  
Standard pre PKI a podpisovanie MIME dat  
End-to-End sifrovanie

Hlavicky

- `application/pkcs7-mime` - zasifrovana cast
- `application/pkcs7-signature` - certifikat

Ponuka

- Digitalny podpis
  - Autentifikaciu
  - Integritu
  - Nepopieratelnost povodu
- Sifrovanie
  - Sukromie
  - Zabezpecenie dat

Vyhody

- Sifrovanie komunikacie
- Autentifikacia odosielatela
- Definicie novych hlaviciek

Nevyhody

- Ziskanie certifikatu
- Niektore protokoly musia pouzit workaround

### TLS

TLS a jeho predchodca SSL sluzia na sifrovanie dat

TLS 1.2 - RFC 52465  
TLS 1.3 ([draft](https://tools.ietf.org/html/draft-ietf-tls-tls13-18))

Zalozene na certifikatoch (asymetricke sifrovanie)  
Sifrovanie hop-by-hop

Princip

- 2 fazy
  - TLS Handshake Protocol
  - TLS Record Protocol

Bali sa do TCP

TLS Handshake

- Klient posle Hello (typy TLS verzii, podporovanie sifry, ...)
- Server posle Hello (vybrana verzia TLS, alg., ...) a certifikat
- Klient posle pripadny certifikat a _pre-master heslo_ zasifrovane verejnym klucom
- Generovanie zdielaneho hesla
- Klient posle _Change cipher spec_ a _Client finished_
- Server posle _Server finished_

Komunikacia zabespecena symetrickym sifrovanim s novy zdielanym heslom

Vyhody

- Sifrovanie komunikcie
- Autentifikacia odosielatela

Nevyhody

- Ziskanie certifikatu
- Jemne zatazenie systemu

### DTLS

Datagram Transport Layer Security

Zabezpecuje bezpecnost pre datagramove protokoly (UDP, DCCP, SCTP)  
RFC 4347  
RFC 6347  
Postavene na TLS - `DTLS 1.0` = `TLS 1.1`, `DTLS 1.2` = `TLS 1.2`  
Aplicacia si zabezpecuje usporiadanie paketov a straty

Problemy

- TLS nedovoluje desifrovanie lubovolne datagramu (Cipher Block Chaning)
  - Riesenie - zakaz prudovych sifier
- TLS Handshake spravy su posielane spolahlivo
  - Extra casovac na _ClientHello_
- Moze dojst k fragmentacii
  - Srpavy su navrhnute malej velkosti
  - Obsahuju _fragment offset_ a _fragment length_
- Reordering
  - Obsahuje sequence number

Kde sa mozeme stretnut

- Cisco AnyConnect VPN Client
- f5 Networks Edge VPN Client
- Chrome, Opera, Firefox - pre WebRTC

Prelomeny - februar 2013

### IPsec

Rodina protokolov popisujucich sposob bezpecneho prenosu IP paketov

Poskytuje

- Utajenie obsahu
- Integritu dat
- Autentifikaciu odosielatela

Pouziva protokoly

- IKE - Internet Key Exchange - vymena klucov
- AH - Authentication Header - autentifikacia
- ESP - Encapsulating Security Payload - utajenie obsahu

AH

- Chrani cely obsah paketu (aj hlavicku)
- Nesifruje
- Poskytuje autentifikaciu a kontrolu integrity
- Volitelne poskytuje ochranu proti opakovaniu (replay detection)

ESP

- Sifruje paket
- Nesifruje hlavicku
- Ponuka vsetky sluzby AH (autentifikacia, integrita, anti-replay)

AH sa pouziva zriedkavo, ESP casto  
Obe sa do IP paketu pridavaju ako pridavne hlavicky

Prenosove rezimy

- Transportny rezim - ponecha povodnu IP hlavicku
- Tunelovy rezim - prida novu IP hlavicku

Bezpecnostna asociacia (SA)

- Jednosmerna relacia vytvorena pri kazdom IPsec spojeni
- Virtualne spojenie dvoch zariadeni
- Obsahuje vsetky informacie spojenia
  - Typ protokolu (ESP, AH)
  - Rezim prenosu (transportny, tunelovaci)
  - Sifrovaci algoritmus (NULL, DES, 3DES, AES)
  - Autentifikacny algoritmus (HMAC-MD5, HMAC-SHA1)
  - Doba zivotnosti

Sprava SA

- Rucna konfiguracia
  - Minimum problemov
  - Zla skalovatelnost
  - Neumoznuje casto menit kryptograficke kluce
  - Minimalne pouzitie
- Automaticka konfiguracia
  - Internte Security Association and Key Mangement Protocol (ISAKMP)
  - RFC 2408
  - Odporucane

### MACsec

Zabezpecenie technologie Ethernet (L2)  
Rozsirenie 802.1X  
IEEE 802.1AE

Zabezpeci komuniakiu medzi zariadniami (hop-by-hop)  
Prevadza na backplane je nezabezpecena

Ponuka

- Autentifikaciu
- Integritu dat
- Dovernost (confidentiality)

### IEEE 802.1X

Protokol pre pristup do pocitacovej siete  
Vyuzivany pri drotovej aj bezdrotovej sieti  
Pokial sa klient neautentifikuje, prevadzka je zahadzovana  
Pracuje na L2  
Vyuziva protokoly Radius/Diameter

Princip

1. Klient sa pripoji k zariadeniu (switch/AP)
2. AP akceptuje len EAP ramce, ostatne su zahadzovane
3. Klient odosle autentifikacne udaje cez EAP
4. AP preposle udaje Radius serveru
5. Radius server overi udaje - lokalne aleo na Radius serveri v klientovej domovskej sieti
6. Radius server odosle informacie AP

Vyhody

- Blokovanie neautorizovanych osob
- Umiestenie zariadenia do specifickej VLAN

Nevyhody

- Chrani len pristup k sieti

### Extensible Authentication Protocol (EAP)

Autentifikacny framework, nie samotny mechanizmus  
Najcastejsie pouzivany v bezdrotovych sietach (WPA/WPA2)  
Zaistuje zjednanie autentifikacnych metod (okolo 40) - `EAP-TLS`, `EAP-PSK`, `EAP-MD5`, ...  
Nie je protokol, len definuje format sprav, nie je zapuzdrovany
