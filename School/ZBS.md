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
