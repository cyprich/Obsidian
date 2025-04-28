# ZBS

Poznamocky z predmetu Zaklady Bezdrotovych Sieti

## Historia

Prva teoria elektromagnetizmu - 1831 - Faraday  
Teoria pouzita v praxi - 1864 - Maxwell  
Prvy vyslany radiovy signal - 1870 - Mahlon Loomis  
Prvy dialkovy bezdrotovy hovor - 1915

## Zaklady

### Ziarenie

- Ionizujuce
  - Ionizacia = proces pri ktorom atom/castica straca/ziskava elektrony = meni sa na ion
  - Patri sem ultrafialove ziarenie, rontgenove ziarenie, gama ziarenie
  - Moze pochadzat z
    - Kozmicke - zo slnka - zvysuje sa s nadmorkou vyskou
    - Z hornin a pody - uran, torium, draslik, radon
    - Podzemna voda - je v nej rozpusteny radon
    - Umele - rontgen, urychlovace, ziarice
  - Moze sposobit popalenie, chorobu z oziarenia, geneticke poskodenie
  - Chranit sa voci nemu da vzdialenostou, casom, tienenim
- Neionizujuce
  - Patri sem viditelne svetlo, infracervene, mikrovlnne, radiove vlny, nizkofrekvencne polia vytvarane elektrickymi pristrojmi a vedeniami
  - Bezne nas obklopuje, nesposobuje zmenu v atomoch (nedostatocna energia) -> znacne nizsie riziko ohrozenia
  - Rizika
    - Viditelne svetlo - priamy pohlad moze poskodit sietnicu
    - Tepelny efekt (popaleniny) vysokofrekvencneho ziarenia (infracervene, mikrovlnne)
    - Pri beznom vystaveni je bezpecne

Elektromagneticke vlnenie (EM) je jav, pri ktorom sa elektromagneticka vlna prenasa od zdroja k spotrebicu  
Je dane frekvenciou `f [Hz]`

Magneticke pole - magneticka indukcia `B [T]` _(Tesla)_

Elektricke pole - intenzita `E [V/m]` _(voltmeter)_

### Pojmy suvisiace so sirenim riadovych vln

- Ohyb (Difrakcia)
- Lom (Refrakcia)
- Rozptyl (Scattering)
- Odraz (Reflection)
- Prienik (Transmission)

### Fresnelova zona

Pre kazdu vysielanu frekvenciu sa da definovat rozhranie, ktore tvori hranicu Fresnelovej zony v tvare '3D elipsy'  
Moze ich byt lubovolen vela  
Najdolezitejsia je 1. Fresnelova zona - najblizsie k sponici medzi vysielacom a prijimacom - siri sa v nej najvacsia cast (60%) EM vlnenia

Polomer Fresnelovej zony (v metroch) vypocitame ako $r = \sqrt{\dfrac{d \cdot \lambda}{4}}$, alebo $r = 8.657 \cdot \sqrt{\dfrac{d}{f}}$

> $d$ = vzdialenost vysielaca a prijimaca \[m]  
> $\lambda$ = vlnova dlzka \[m]  
> $f$ = frekvencia \[GHz]

Vzdialenost dotyku 1. Fresnelovej zony od povrchu zeme $d \cong \dfrac{4 \cdot h_{zs} \cdot h_{ms}}{n \cdot \lambda}$

> $zs$ = zakladova stanica
> $ms$ = mobilna stanica
> $h$ = vyska \[m]
> $d_1$ = priama vzdialenost \[m]
> $d_2$ = odrazena vzdialenost \[m]
> $\lambda$ = vlnova dlzka \[m]
> $n$ = cinitel
> Zmena koeficientu tlmenia nastava az pri tieneni viac ako 55% 1. Fresnelovej zony

### Definovanie zakladnych velicin

#### Vykon

Mnozstvo prace `W` vykonanej za jednotku casu `t`

Znacka `P`, jednotak `W` (Watt)

$P = \dfrac{W}{T}$

#### Elektricky vykon

Praca elektrickeho prudu vykonana za jednotu casu

$P = U \cdot I$

#### Vyzarovaci vykon - EIRP

Effective Isotropic Radiated Power  
Vykon, ktory by musela vyziarit hypoteticka izotropna antena, aby bolo dosiahnute rovnakej urovne signalu v smere maximalneho vyzarovania danej anteny _(???)_

Maximalny vyzarovaci vykon je dany normami

- 2.4GHz Wi-Fi - $100mW$ = $20 dBm$
- 5GHz Wi-Fi - $200mW$ = $23 dBm$

Vypocet: `Vykon vysielaca [dBm]` + `zisk anteny [dBi]` - `utlm kabla [dB]` - `utlm konektorov [dB]`

#### Decibel (`dB`)

**Relativny** udaj o dvoch roznych urovniach vykonu  
Udava pomer
Pouziva sa na vyjadrenie zisku/straty jedneho zariadenia ($P_2$) vo vztahu k druhemu ($P_1$)

$A/G = 10 \log \left( \dfrac{P_2}{P_1} \right)$

##### Decibel miliwatt (`dBm`)

Logaritmicka jednotka vykonu  
Vztahuje sa k vykonu `1mW`

##### Decibel Watt `dBW`

Logaritmicka jednotka vykonu  
Vztahuje sa k vykonu `1W`

##### Energeticky zisk izotropnej anteny (`dBi`)

## WLAN

1. PAN - Personal (Bluetooth)
2. LAN - Local
3. MAN - Metropolitan
4. WAN - Wide

IEEE 802.11 WLAN

Prvy 802.11 standard v roku 1997

Dve hlavne frekvencie

- 2.4 GHz
- 5 GHz

> 6 GHz - WiFi6E a WiFi7

Organizacie

- ITU - International Telecommunication Union (1865)
- IEEE - Institute of Electrical and Electronics Engineers
  - Rodina standardov `802`, napr. `802.11`, `802.3` atd. atd.

WLAN network topologies

- Infrastructure mode
  - Umoznuje pouzit controller - jednoduchsie manazovanie viacerych AP
- Ad-hoc mode
  - V podstate hotspot na mobile?

### IEEE terminology

- AP - Access Point
- STA - Station - koncove zariadenie
- BSS - Basic Service Set - zariadenia pripojene na jedno AP
- ESS - Extended Service Set - zariadenia pripojene na 2+ APs
- IBSS - Independent Basic Service Set - Ad-hoc
- SSID - Service Set ID - textove ID
- BSSID - Basic Service Set ID - MAC adresa ako ID
- DCF - Distributed Coordination Function - riesenie kolizii pomocou nejakeho time slotu (dnes namiesto toho CSMA/CA)

### L1 podla IEEE 802.11

2.4 GHz channels

- Sirka kanalu 22 MHz
- Odstup jednotlivych kanalov 5 MHz -> prekryvaju sa
- Neprekryvajuce sa kanaly - `1`, `6`, `11`
- 13 kanalov v EU, 14 v Japonsku, 11 v US

5 GHz channels

- Odstup jednotlivych kanalov je tiez 5 MHz
- Pouzivaju sa kazde 4 kanaly (`36`, `40`, `44`) co poskytuje 20MHz kanaly ktore sa neprekryvaju

#### Modulacia

- Analogova
  - FM - Amplitudova
  - AM - Frekvencna
  - PM - Fazova
- Digitalna
  - FSK - Frequency key shifting
  - ASK - Amplitude key shifting
  - PSK - Phase key shifting
    - BPSK - Binary Phase Shift Keying - otocime fazu o 180 stupnov
    - QPSK - Quadrature Phase Shift Keying - otocime fazu o nasobok 45 stupnov
    - QAM - Faza + amplituda - QAM-16, QAM-64, QAM-256

#### Znizenie interferencie

- FHSS - Frequency Hopping Spread Spectrum
  - (Pseudonahodne) Rychle prepinanie kanalov
- DSSS - Direct Squece Spread Spectrum
  - Encoding with 11-chip Barker sequence
  - Used only at 1 or 2 Mbps
- CCK - Complementary Code Keying
  - Set of 4 or 64 8-bit code words (symbols) used to encode data for 5.5 or 11 Mbps
- OFDM - Orthogonal Frequency Division Multiplexing
  - The information is carried using orthogonal subcarriers
  - While one signal is at it's peak, others are at 0
  - Allows to pack more data into a smaller range of frequency

### MCS

Modulation Code Schemes

### Multiple Access Protocols

> 3.3.2025

- Random Access Protocols
- \_ Access Protocols
- Channelized Access Protocols

#### ALOHA

Ak nevies ci niekto rozprava tak povedz, uvidis

#### CSMA

Carrier Sense Multiple Access

- CSMA/CD - Collision Detection
  - Pocuvame ci niekto nevysiela
  - Ak nikto nevysiela, tak mozem vysielat ja
- CSMA/CA - Collision Avoidance
  - Kolizia se nedetekuje, iba sa im vyhybaju (resp. minimalizuju pravdepodobnost)
  - Chceme vyslat ramec, ale este predtym pocuvame ci prebieha nejaka komunikacia
    - Ak prebieha, tak pockame nejaku dobu a skusime dalej
    - Ak neprebieha,
- Token Ring - uz sa nepouziva
  - Medzi zariadeniami sa posuva "zeton"
  - Kto ma zeton moze vysielat, potom posle zeton dalej
- Token Bus - uz sa nepouziva
  - Podobne ako Token Ring, ale zeton je virtualny

##### RTS/CTS

Request to Send/Clear to Send  
Ziadame si povolenie o vysielanie

---

Hidden Node problem

- Povedzme ze mame 3 nodes - `A`, `B`, `C`
- `B` je v strede medzi `A` a `C`
- `B` vidi aj `A` aj `C`
- `A` ale nevie o `C` a zaroven `C` nevie o `A`

---

NAV - Network Allocation Vector

---

### Authentication and Association

1. Unauthenticated & Unassociated
1. Authenticated & Unassociated
1. Authenticated & Asssociated

Authentication - posleme poziadavku, AP si zaeviduje a povoli nam  
Association -

## Mobile communications

Generacie

- 1G (NMT - Nordic Mobile Telephone) - 1980s - Analog 2kbps
- 2G (GSM - Global System for Mobile) - 1991 - 270 kbps
- 3G (UMTS - Universal Mobile Telecommunication System) - 1998 - 3Mbps
- 4G (LTE - Long Term Evolution) - 2008 - 1Gbps
- 5G (NR - New Radio) - 2020 - 10Gbps

Pokles/utlm signalu (exponencialny) - decibely - 10dB = jeden rad

### 1G - Nordic Mobile Telephone

Vznikli v Nordic krajinach (Norsko, Svedsko) kde bola ich potreba  
Ericson, Nobira (Nokia)  
Analogova komunikacia, FM modulacia  
Ziadne SIM, zle zabezpecenie, vacsinou nie pre beznych pouzivatelov?

V amerike AMPS, moc sa neuchytilo  
NTT v Japonsku

U nas Eurotel - rozdelil sa na Slovak Telecom a O2 v Cesku

25KHz kanaly, 200 uplikov, 200 downlinkov

Architektura

- Mobile station (MS)
- Base station (BS)
- Mobile Telephone Exchange (MTX)
- Home Location Register (HLR)

Rozdelenie na oblasti - vacsinou 4 alebo 7

### 2G - Global System for Mobile

Vzniklo vo Finsku, 1991, ETSI
Prvy digitalny  
Urcene na SMS, nie na prenost dat, bola zvysena security oproti 1G  
Rozsirenie sluzby o prenos dat - od roku 2000 - to je to ked je na mobile pri datach `E`

IMSI (International Mobile subscriber Identity) - jedinecny identifikator, sklada sa z

- MCC (Mobile Country Code) - pre slovensko `421`
- MNC (Mobile Network Code) - podla operatora: T-Mobile (2, 4), Orange (1, 5, 15), ...
- MSIN (Mobile Subscriber Identification Number)

[esims.io](esims.io)

Conventional vs. Distributed BTS site  
BTS components: Duplexer, Diplesers (splitter/combiner), MHA (Mast Head Amplifier)

Frekvencne pasma GSM-900, GSM-1800, GSM-1900, GSM-850

GSM speech coding - Full Rate, Half Rate

GSM modulation - FSK, MSK

Protocol Stack - uplne ine protokoly ako v normalnych IP sietach

Authentication triplet (RAND, SRES, Kc)

### 2.5G - General Packet Radio Service

### 3G - Universal Mobile Telecommunication System

3GPP - 3rd Generation Partnership Project

V podstate len rozsirenie GSM  
Zasadna zmena - CDMA Principle - umoznuje radovo vyssie rychlosti

### 4G - Long Term Evolution

Zasadne ine od ostatnych  
Modulacia - OFDMA  
Uz sa viacej podoba IP sietam

### 5G - New Radio

## Bezpecnostne normy

Normy, standardy, usmernenia

Eletrosmog  
Vlyv na zive organizmy - umele zdroje ziarenia  
Vela studii aby sa mohol spravit standard

Tepelne a netepelne ucinky  
Dokazane iba tepelne ucinky, ziadne straty pamate ani nic podobne neoveboli overene

Norma - oficialny dokument schvaleny normalizacnou aturoitou  
Standard - odporucanie  
WHO, ICNIRP, Narodne regulacne urady

Akcne hodnoty,

Zakon, vyhlaska, nariadenie, metodika

2 skupiny, pracovnici, obyvatelstvo  
Objektivizacia expozicie EM polom

---

Vplyv EM pola na cinnost elektronickych zariadeni  
Kardiostimulatory, nemocnice - prisnejsie normy

## Beezdrotove technologie pre IoT

Bunkove siete (GSM, LTE) - licencovane pasma

- EC-GSM-IoT, LTE-M, NB-IoT, 5G IoT

Low-power siete - ISM (industrial, scientific, medical) Pasmo

- BLE, Bluetooth Mesh (WPAN) - najmenej priestorovo rozlahle (geograficky)
- IEEE 802.15.4, LR-WPAN, ZigBee - stredna velkost
- LoRa, LoRaWAN - najviac rozhlahle
- ...

**IoT** - v com sa lisi od "normalneho" internetu

> Veci bezneho pouzitia pripojene do internetu

V normalnom - Asymetricke pripojenie - smerom ku nam je preferovana rychlost (upload)  
V IoT je preferovany download

IoE - veci, procesy, ludia, zvierata  
WSN - Wireless Sensor Networks

Pocet "veci" v roku 2030 - 24 miliard

Suvisiace technologie s IoT

- Pokrocile metody analyzy dat
- Strojove spracovanie dat (AI)
- Edge computing - odlahcenie cloudoveho spracovania dat - spracovavanie tam, kde data vznikaju

Obmedzenia "veci"

- Velkost
- Vykon
- Spotreba
- Dosah
- Rychlost

### EC-GSM-IoT

Low Power Wide Area Network - LPWAN  
2G siete, do 50 kbps  
Smart merace, zabudovane senzory, asset trackers  
Dobra zivotnost baterii (roky)  
Takmer globalna pouzitelnost (zhorsuje sa)  
Kompatibilita s mnozstvom zariadeni  
Nizka cena (zariadenia aj data)  
Prestava sa pouzivat (EU do 2025?)

### LTE

4G siete  
Teoreticky 50 Mbps upload, 150 Mbps download  
Softwarovo definovane radio - flexibilita v podpore novych standardov

Standardy pre IoT

- NB IoT - Narrow Band IoT
- LTE-M
- Cat-1
- Cat-4

#### NB IoT

Low-cost, Low-power - 10 rokov na 2 AA baterie  
200kHz pasmo, moze vuyzivat guard band v LTE - prazdne pasmo aby sa zamedzilo interferencii v LTE  
Dobre pokrytie v budovach a podzemi  
Dosah az do 10 km  
Upload 66 kbps, download 26 kbps, verzia 2 ma vyssie rychlosti  
Latencia 1.6 az 10 sekund, dlzka spania max 3 hod  
Len pre stacionarne zariadenia, nie roaming, nie tower handover  
Meranie spotreby (voda, elektrika, plyn), smart city (osvetlenie, parkovanie), monitorovanie v priemysle, polhonospodarstve

#### LTE-M

LTE-machine - urcene pre stroje  
Podporuje mobilne zariadenia  
Pasmo, 1.4 MHz, half-duplex aj full-duplex  
1 Mbps, zvycajne upload 380 kbps, download 300 kbps  
Latencia 10-15 ms, max dlzka spania 40 min  
Podpora pre mobilne zariadenia - asset tracking, fleet management  
Podpora pre hlasove aplikacie - medical alert devices, home alarm systems  
Smart meters, industrial monitors, asset tracking, health monitor, alarms, wearables  
Nie je dostupne globalne

#### Cat-1

Starsie  
Vyssia spotreba, mensi dosah, drahsie  
Lepsie globalne pokrytie  
20MHz pasmo, upload do 5 Mbps, download do 10 Mbps  
Latencia 50-100 ms, full duplex  
Podpora pre hlas, mobilne zariadenia  
Nositelnosti, kiosky, video dohlad, starostlivost o zdravie, bankovamty, zdielana mobilita - prenajom bicyklov a kolobeziek, autonomne drony na dorucovanie

#### Cat-4

20 MHz pasmo  
50 Mbps upload, 150 Mbps download  
Najdrahsie  
Video dohlad, video aplikacie v realnom case, in-car hostspot, ...  
8

### 5G

Ultra low-latency (vhodne pre priemyselnu automatizaciu)  
1 Gbps  
Podpora velkeho mnozstva zariadeni  
Aplikacie

- Smart cars
- Smart cities
- Biznis  
  Zdravotnictvo

### Bluetooth

802.15 - WPAN

- .1 - Bluetooth
- .4

Povodny ciel - nahrada drotoveho spojenia na kratku vzdialenost  
Vyvijane firmou Ericsson  
V sucastnosti spravovane zdruzenim Special Interest Group (SIG)  
Komercne dostupne zariadenia od roku 2001 - Ericsson T39 a IBM ThinkPad A30  
Rozsireny aj vdaka Motorole

Meno podla danskeho krala Heralda Bluetootha

Kratky dosah - do 10 metrov  
Nizka spotreba, low-cost  
ISM pasmo - 2.4 az 24835 GHz  
79 kanalov so sirkou 1 MHz  
FHSS - Frequecy-Hopping Spread Spectrum - 1600 hopov za sekundu  
Modulacia - GFSK (Gaussian Frequency-Shift Keying), DQPSK (Differential Quadrature Phase-Shift Keying), DPSK  
Apple HDR4 a HDR8 - 4MHz kanaly s FEC

Komunikacia zalozena na packetoch  
Master/Slave architektura, hviezdicova struktura  
Max. 7 slaves, v novsej verzii az 14 (na 2 logickych kanaloch)  
Piconet - jedna ako keby BT siet  
Scatternet - prepojenie viac piconetov  
Synchronizacia Mastrom - kazdych 312.5 us  
Slot 625 us  
Par 1250 us  
Paket - 1, 3, alebo 5 slotov  
Master zacina v parnom, Slave v neparnom slote  
Moznost vymeny uloh Master-Slave  
V jednom case komunikacia len s jednym - zvycajne round-robin

Vykon dBm, mW

Bluetooth profil

- Definuje, na co je urcene dane zariadenie
- Nastavenia - parametrizacia a riadenie komunikacie
- Jednoduche vytvorenie spojenia
- Napr. A2DP, HID, HFP, HSP, SSP, ATT, GATT, prenos suborov, remote control, tlac, video, LAN, Mesh, proximity

Verzie 1.0 (2000), 2.0 (2005), 3.0 (2009), 4.0 (2010, aj BLE), 5.0 (2016), 5.4 (2023)

### Bluetooth Low Energy

Urceny viac na IoT  
Pre systemy napajane bateriami - malo prenasanych dat, nie prilis casto  
Energiu zerie aj vysielanie aj prijimanie

Nekompatibilne s BT Classic  
40 kanalov - 2 MHz  
Discovery na 3 kanaloch (32 BT Classic)  
Spatna kompatibilita

Long-range vs. High-speed

BLE je asymetricke - vacsina prace je na Mastrovi (riadenie spojenia, casovanie, spracovanie dat)

Gombikova bateria >1rok  
Male pakety - 244B  
Kratke vysielacie a prijimacie okna

V porovnani s konkurenciou

- Nizsia spotreba
- Specifikacia zadarmo
- Lacnejsie moduly a cipsety
- Pritomnost vo vacsine smartfonov

Podporuje aj prenos audia, many-to-one aj one-to-many (v5.2)

#### Hlavne pojmy

Cinnosti

- Inzerovanie (advertising) - pravidelne vysielanie informacii
- Skenovanie - hladanie inzerujucich zariadeni

Typy zariadeni

- Periferia - inzeruje a umoznuje pripojenie, zdroj dat
- Centrum - skenuje a moze sa pripojit, spotrebic dat
- Observer - skenuje, pocuva, ale nepripaja sa
- Broadcaster - inzeruje, ale neumoznuje pripojenie (majak)
  - Kazdy moze pocuvat
  - Nespolahlivy prenos

#### Stavy BLE zariadenia

Periferia

- Stand-by
- Inzerovanie

Centrum

- Skenovanie
- Iniciovanie
- Pripojenie

### Linkova vrstva

Poskytuje abstrakciu fyzickej vrstvy pre vyssie vrstvy  
Inzerovanie, skenovanie, vytvaranie a udrzovanie spojeni

### Bluetooth Mesh

Mesh - topologia many-to-many  
2017, zvlast standard  
Podpora BLE od 4.0, vyzaduje kompletny BLE stack  
Samolieciaca schopnost (self-healing) - ak nejake zariadenie vypadne, siet by sa mala sama prisposobit

Architektura BT Mesh

1. Bearer layer
2. Lower transport layer
3. Upper transport layer
4. Access layer
5. Foundation models layer
6. Models layer
7.
8.

#### Terminologia

Unprovisioned device

Nod

- Nod - zakladny typ
- Relay nod - dokaze preposielat spravy
- Proxy nod - komunikacia so zariadeniami bez BLE
- Low power nod - obmedzene napajanie, musi mat priatela
- Friend nod - priatel LPN, uchovava pre neho spravy

Elementy

Stavy

Vlastnosti

#### Spravy

GET  
SET  
STATUS

#### Model

Server model  
Client model  
Control? model

#### Adresy

Unicast  
Multicast  
Virtualna - jeden/viac elementov v jeden/viac nodoch

#### Publish-subscribe

#### Provisioning

Pridavanie zariadenia do siete
