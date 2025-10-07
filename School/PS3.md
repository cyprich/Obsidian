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
