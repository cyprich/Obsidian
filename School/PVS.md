# PVS

Prepojene Vstavane Systemy

## Organizacia

celkovo 60 zo 100  
cvicenia (semestralka, aktivita - 25+5) 15 z 30  
test (moodle - teoria) - 50  
ustna skuska (nepovinna) - 20  
bonusove body na prednaske - 10

## Uvod

Zakladne casti PC

- CPU - riadiaca + aritmeticko - logicka jednotka
- Vstupno-vystupna jendotka
- Pamat

Pracovny cyklus PC

- Vyber instrukcie z pamate - fetchs
- Dekodovanie instrukcie - decode
- Vyber operandov - read
- Vykonanie operacie - execute
- Zapis vysledku - write

Su na sebe nezav_sle - prudove spracovanie - pipeline  
Priklad 1GHz CPU - normalne by mohol vykonavat len 200M instrukcii, vdaka pipeline moze actually 1G

2 najpouzivanejsie architektury

- Von-Neumann - spojena pamat pre program a udaje
- Harvard - pamat zvlast pre program, zvlast pre udaje
  - Vyuziva sa hlavne vo vstavanych systemoch
  - Vyhody
    - 2 rozne komunikacne cesty - zbernice (bus)
    - Moze byt uplne odlisny typ pamate - ROM (flash pamat, NVRAM) a RAM

Single Instruction Single Data (SISD)

- 1 CPU (jedno jadro), vo vstavanych systemoch

Multiple Instruction Multiple Data (MIMD)

- Viac CPU (viac jadier), zdielana pamat, pouziva sa v modernych systemoch

### Vstavany system

System s pocitacom - stroj, elektricky spotrebic, budova, oblecenie

Ulohy

- Hlavna cinnost - meranie, riadenie, zaznam dat
- Komunikacia - pouzivatel, iny system, cloud

Obsluha zariadeni

- Pravidelna kontrola - tlacidlo, obsah registra, ... - jednoduche, ale zbytocne zatazuje, vacsinu casu sa nic nedeje
- Prerusenia
  - Podprogram sa zavola sam automaticky ako odpoved na udalost (stlacenie tlacidla, timer) - nieco ako funkcia
  - Rychlejsie, efektivnejsie
  - By default vacsinou je zakazane "prerusenie prerusenia", ale dalo by sa zapnut ak vieme co robime
  - Podprogram by nemal byt moc velky, zlozity, nemal by mat loopy, ked sa zacykli tak koniec sveta
  - Priebeh prerusenia
    - Prijatie poziadavky na prerusenie
    - Dokoncenie rozrobenej instrukcie
    - Odlozenie okamziteho stavu procesora
    - Zistenie zdroja prerusenia
    - Vykonanie zodpovedajuceho obluszneho programu prerusenia
    - Obnovenie povodneho stavu procesora
    - Pokracovanie v prerusenom programe
  - Rozdelenie/typy
    - Vnutorne, softverove, **hardverove**
    - Niektore su maskovatelne (daju sa zakazat)
    - Synchronne/**asynchronne**
    - Rozna priorita

### Operacny system

Rozhranie medzi hardwarom a softwarom  
Spravuje zdroje pocitaca (CPU, pamat, IO, komunikacne rozhrania (BT, seriovy port, I2C SPI, ...), prevodniky, casovace, ...)

**Proces** - vykonavany (beziaci) program (aplikacia)  
**Vlakno** - podprocesy - samostatne/paralelne vykonavane cinnosti  
Proces = 1+ vlakien  
**Uloha** - task - spolocny nazov pre vlakno aj proces

## RTOS

## Vstavane Systemy

Vnorene, zabudovane, embedded systems  
Pocitac zabudovany do systemu (vyrobnku) s cielom zlepsit vlastnosti alebo pridat nove funkcie (smart TV, biela technika, auta, wearables, ...)

### Historia

### Jadro vstavanych systemov

Pocitac

- Jednoucelovy pocitac - na mieru - neefektivne, drahe, neflexibilne
- Jednodockovy pocitac - raspberry pi, banana pi, beagle board
- PLC - Programovatelne Logicke Kontrolery - kompaktne, robustne, odolne, modularne, spolahlive, drahsie
- Miktokontolery - arduino, esp32, raspberry pi nano - treba dorobit napajanie, periferie; zvycajne male, lacnejsie
- Digitalne signalne procesory - podpora spracovania obrazu, zvuku - hybridne systemy
- FPGA - programovatelne logicke polia - definicia hardwaru "na mieru" - hybridne systemy
- ASIC - application specific integrated circuit - vlastny navh cipu pre velke serie

Sucasny trend

- Modularne systemy - PLC, cRIO, ... - nahradne diely, dlhodoba zaruka
- Podobne pri mensich projektoch - Arduino

### Kapacita prenosoveho kanala

Vyuzitie = $\dfrac{T_M}{T_B}$, kde $T_M$ je trvanie najkratsieho impulzu a $T_B$ je trvanie bitu

### Kody

RZ - Return to Zero - s navratom k nule
NRZ - No Return to Zero

**Unipolarny RZ kod** - iba jeden pol - iba kladne  
Problem so synchronizaciou je rieseny automaticky (samosynchnonizujuci) - obsahuje hodiny - na zaciatku kazdeho bitu sa zmeni z `0` na `1`  
Platime za to nizsim vyuzitim kapacity $K = 33\%$

![obrazok](pvs-unipolarny-rz-kod.png)

**Bipolarny RZ kod** - kladne aj zaporne  
Na zaciatku je tiez zmena - ak do kladneho tak je `1`, ak do zaporneho tak je `0`  
$K = 50\%$

![obrazok](pvs-bipolarny-rz-kod.png)

**AMI kod** - Alternate Mark Inversion  
`0` je vzdy na nule (0V)  
`1` sa strieda - raz hore, raz dole  
$K = 100\%$  
Problem so synchronizaciou pri nulach - vyriesime tak ze tam dame `1` (na druhej strane ju musim potom odstranit) - napr. sa dohodneme ze "max. 5 nul"

![obrazok](pvs-ami-kod.png)

**NRZ** - bez navratu k `0`

**Unipolarny NRZ**  
Napatie = `1`  
Bez napatia = `0`  
$K = 100\%$

![obrazok](pvs-unipolarny-nrz-kod.png)

**Bipolarny NRZ**  
Napatie = `1`  
-Napatie = `0`  
$K = 100\%$

Obidve ma problem so synchronizaciou aj pri `0`, aj pri `1`

![obrazok](pvs-bipolarny-nrz-kod.png)

**NRZ space**  
`0` = zmena na zaciatku bitu  
`1` = bez zmeny na zaciatku bitu  
Pouziva sa napr. v USB  
Sync. - dlhy sled `1`  
$K = 100\%$

![obrazok](pvs-nrz-space.png)

**Kod Manchester**  
`1` = zmena 0 -> 1  
`0` = zmena 1 -> 0  
Manchester = hodiny XOR data  
Pouzite v RFID, NFC, IEEE 802.3 - 10BASE-T)  
$K = 50\%$  
Zmeny v strede bitu

![obrazok](pvs-manchester.png)

**Diferencny Kod Manchester**  
`0` = bez zmeny  
`1` = zmena  
Odolne voci prepolovaniu  
Teraz zmeny na zaciatku bitu  
Pouzite v Token Ring LAN, ukladanie dat  
$K = 50\%$

![obrazok](pvs-diferencny-manchester.png)

**Fazova modulacia** - FM  
`0` = zmena na zaciatku bitu  
`1` = zmena v strede bitu  
$K = 50\%$

![obrazok](pvs-fm.png)

**Modifikovana Fazova Modulacia** - MFM  
Ak po `1` ide `0`, potlacime zmenu, inak rovnako ako FM  
Vyriesime problem s "malymi odsekmi", cim dosiahnemem kapacitu $K = 100\%$

![obrazok](pvs-modifikovana-fm.png)

Sum

| Kod                            | Princip                                                     | Kapacita |
| ------------------------------ | ----------------------------------------------------------- | -------- |
| Unipolarny RZ                  | log. 0 = kratky impulz, log. 1 = dlhy impulz                | 33%      |
| Bipolarny RZ                   | log. 0 = zaporny impulz, log. 1 = kladny impulz             | 50%      |
| Alternate Mark Inversion (AMI) | log. 0 = 0V, log. 1 = striedavo `+U` a `-U`                 | 100%     |
| Unipolarny NRZ                 | log. 0 = OV, log. 1 = `+U`                                  | 100%     |
| Bipolarny NRZ                  | log. 0 = `+U`, log. 1 = `-U`                                | 100%     |
| NRZ Space                      | log. 0 = zmena na zaciatku bitu, log. 1 = bez zmeny         | 100%     |
| Manchester                     | Hodiny `XOR` data                                           | 50%      |
| Diferencny Manchester          | log. 0 = bez zmeny, log. 1 zmena v strede bitu              | 50%      |
| Fazova modulacia               | log. 0 = zmena na zaciatku bitu, log. 1 zmena v strede bitu | 50%      |
| Modifikovana Fazova modulacia  | Ako FM, ale vynechana zmena pri zmene `1` -> `0`            | 100%     |

## Generovanie Logickych Signalov

> Vystup z obvodov

**Push-pull**  
2 spinace (tranzistory)  
Bud je zopnuty jeden, a mame na jednom vystupe napajacie napatie; alebo je zopnuty druhy a mame na druhom vystupe 0V  
Vzdy je zopnuty iba jeden  
Spojenie vystupov sposobi skrat

**Pull-up**, alebo open collector, open drain  
_Wired AND_  
Viac systemov  
Kazdy moze nastavit iba na log. 0  
Log. 1 je iba vtedy ked nikto nenastavi 0  
Zbernica

**Pull-down**  
Opacny princip  
Ked nikto nic = log. 0  
Ked aspon 1 nieco robi = log. 1  
_Wired OR_

### Signalizacia

Sposob prenosu signalu medzi 2 ucastnikmi

**Single ended**  
Jeden vodic - referencny  
Druhy vodic - signalovy  
Vyhody - malo vodicov ($N+1$ pre $N$ vodicov)  
Nevyhody - crosstalk, citlivost na rusenie, moznost nerovnakeho potencialu zeme

**Diferencialna signalizacia**  
Kazdy signal = 2 vodice (krutena dvojlinka)

**Galvanicka izolacia**  
Izolacny transformator  
2 cievky  
Vyhody - jednoduche, pomerne male, aj pre vyssie napatie  
Nevyhody - len pre striedave napatie (premenlive signaly)

**Optoclen**  
Pomocou svetla (LEDka)  
Vyhody - aj pre jednosmerne signaly, velmi male rozmery, vysoka ucinnost  
Nevyhody - potreba zdroja napatia

### Rozdelenie komunikacie

Podla tvaru dat

- Paralelna
- Seriova

Podla

- Bod-bod
- Hviezda
- Zbernica
- Strom
- Mesh

Podla

- Synchronne (s hodinovym signalom)
- Asynchronne (bez)

## RS-232

TIA/EIA 232

Komunikacia medzi

- DTE (Data Terminal Equipment) - terminal, dalekopis
- DCE (Data Circuit-terminating Equipment) - modem

Pouzitie

- Systemova konzola
- Pripojenie modulov - GPS, FR, Bluetooth, ...
- Komunikacia medzi systemami
- Komunikacia s PC
- Debugovanie

Vlastnosti

- Point-to-point
- Single-ended - spolocny vodic (zem) + dalsie signaly -> kratsia vzdialenost
- Zakladny NRZ kod (logicka uroven = napatova uroven)
- Vzdialenost do 15 m (nizkokapacitny kabel do 300 m)
- Simplex alebo duplex
- Rychlosti \[b/s] - najpouzivanejsie 4800, 9600, 115200, ale aj ine
- Log. 0 = `+3V` az `+15V`
- Log. 1 = `-3V` az `-15V`

Najcastejsie nieco 8 bitov

_Bps_ - bits per second  
_Bd_ - Baud - pocet zmien za sekundu

Najcastejsi ramec - **8E1**

- 8 bitov, ktore sa prenasaju
- E - parna parita - doplna pocet jednotiek na parny - dokaze detekovat 1-bitovu zmenu, dnes sa moc nepouziva
- 1 stop bit -

Pri asynchronnej komunikacii musi byt vzdy start bit  
Prijimac riadi tok - ci je mozne komunikovat

RTS - ready to send  
CTS - clear to send

## RS-422

TIA/EIA 422

Diferencialna signalizacia  
Tx+, Tx-, Rx+, Rx-  
Point-to-point, alebo multidrop (max 10 prijimacov)  
Duplex

Rychlost alebo vzdialenost  
10Mbps do 12m, 100kbps do cca 1200m  
Sucin rychlosti a vzdialenosti by mala byt cca konstantna

## RS-485

TIA/EIA 485  
Dvojvodicova alebo stvorvodicova verzia  
Half-duplex (2v) alebo full duplex (4v)  
Point-to-point, multidrop, **zbernica** (single master)  
Max pocet zariadeni na zbernici 32 (s opakovacmi 247)

## CAN

Controller Area Network  
Zbernica pre automobilovy priemysel

Zbernica  
Multi master  
Async  
NRZ  
MSB first - most significant bit  
Half-duplex  
1Mbps do 40m, 125kbps do 500m  
Potvrdzovana komunikacia  
Kontrola integrity ramca - CRC  
Message-based  
Viac zariadeni = potreba adresacie (je to podobne ako adresa, ale nie je to adresa - identifikacia spravy - ID)  
ID 11 alebo 29 bitov  
Nizsie ID = vyssia priorita

Max 8B dat

Konflikty pri prenose - bezstratova arbitracia  
Jeden zacne rozpravat az ked je ticho  
Ked zacnu dvaja rozpravat, nedojde k poskodeniu dat - ID - vyssia priorita vyhrava  
Synchronizacia - bit stuffing - resynchronizacia pri kazdej zmene 1-0

## SPI

Serial Peripheral Interface

Vlastnosti

- Synchronne
- Single-ended
- Master-slave
- Full duplex
- Vysoka rychlost
- Rozna sirka slova

Vyhody

- Vysoka priepustnost
- Rozna sirka slova
- Full duplex
- Jenoduchy HW
- Jednosmerne signaly
- Jednoducha SW implementacia

Nevyhody

- Vacsi pocet vodicov
- Chyba riadenie toku
- Nepotrvrdzovana komunikacia
- Len 1 master
- Kratka vzdialenost
- Bez kontroly chyb
- Chyba pevny standard

Vyvynute firmou motorola

Pouzitie

- Snimace
- Prevodniky, kodeky
- Pamate
- ...

MISO - Master In Slave Out  
MOSI - Master Out Slave In  
CLK - Clock  
CS - Chip Select

Mody prenosu

| Mod | Polarita | Faza |
| --- | -------- | ---- |
| 0   | 0        | 0    |
| 1   | 0        | 1    |
| 2   | 1        | 0    |
| 3   | 1        | 1    |

Polarita - uroven hodin v pokoji  
Faza - okamih zmeny nacitania dat - `0` = prva hrana citanie, druha hrana zmena dat; `1` opacne

## I2C

Inter-Integrated Circuit

Komunikacia medzi integrovanymi obvodmi  
Dosah max. niekolko metrov

Oznacenie $I2C$, $IIC$, $I^2C$

Pouzitie

- Nizko-rychlostne prevodniky
- Snimace - magnetometer, akcelerometer, gyroskop
- V PC - monitorovanie teploty, rychlosti otacok, komunikacia s monitorom, citanie konfiguracie RAM modulov

Vlastnosti

- Synchronne
- Single-ended
- Multi-master
- Zbernica
- Half-duplex
- MSB First
- Rychlosti - low 10kbps, standard 100kbps, fast 400kbps

Vyhody

- Potvrdzovana komunikacia
- Moznost riadenia toku dat

Nevyhody

- Pomerne malo adries (7bit)
- Nizsia rychlost
- Moznost znefunkcnenia zbernice lubovolnym zariadenim

Prepojenie zariadeni

- SDA - Serial Data
- SCL - Serial Clock
- Zapojenie typu otvoreny kolektor (wired AND)

Struktura ramca

- Start bit
- Adresa 7bit
- RW bit - R = 1, W = 0
- ACK bit
- Data
- Stop bit

Kazdy bajt je potvrdzovany prijemcom  
V jednom ramci mozne preniest viac bajtov  
Kazdy bit je potvrdeny impulzom na SCL  
Master moze dat namiesto stop bitu aj start bit - tzv. restart bit, moze kecat dlho  
Arbitracia - suboj o zbernicu  
Clock stretching - riadenie toku dat

Odvodene standardy - aby sa zabranilo nekonecnemu kecaniu

- SMBUS - System Mangement Bus - v PC - teplomer, otacky ventilatora, ...
- PMBUS - Power Mangement Bus - napajanie - komunikacia so zdrojom
- VESA Display Control Channel - VGA kabel

Neuplna implementacia - TWI - Two Wire Interface - v podstate ako I2C, ale bez closk stretching, ale 99% kompatibilne

## 1 Wire bus

Firma Dallas Semiconductor

Principialne podobne I2C - nizsia rychlost, vyssi dosah  
Prenos dat aj napajania po jednom signali

Pouzitie

- Seriove cisla, pamate, snimace/loggery teploty, RTC
- Identifikacia osob, monitorovanie produktov

Take tie pipaky na otvaranie vchodu

Vlastnosti

- Synchronne
- Single-ended
- Single master
- Point-to-point alebo multidrop (viaceri mozu pocuvat)
- Half-duplex
- LSB First
- Rychlost - standard 16.3kbps, overdrive 10x
- Do 300m - krutena dvojlinka
- Celosvetovo jedinecna 64-bit adresa - typ zariadenia 8bit, ID 48bit, CRC 8bit

Kondenzator - napajanie zariadenia ked je spinac zopnuty (ked niekto posle `0`)

Reset zbernice

- Master posle dlhy `0` impulz - vybije kondenzatory slave-ov
- Zariadenia sa daju do default stavu
- Slaves indikuju svoju pritomnost na zbernici

Prenos dat

- Prenos od mastera
  - Kratky impulz - 15 $\micro$s = `1`
  - Dlhy impulz - 60 $\micro$s = `0`
- Prenos od slave-a
  - Master riadi komunikaciu - hovori kedy zacinaju jednotlive bity
  - Ak slave chce preniest `1`, neurobi nic
  - Ak slvae chce preniest `0`, posle nulu
- Navonok to bude vyzerat tak isto, cize nevieme urcit ze kto komunikuje, iba na zaklade predchadzajucej komunikacie

Komunikacia

- Reset
- Prikaz 8bit - search (enumeracia) slave-ov, selection (vyber) slave-a, broadcast, ...
- Data _N_ \* 8bit

### Search

LSB First

Hladanie zariadeni - S = slave, M = master

1. Prenos bitu adresy (S)
1. Negacia bitu adresy (S)
1. Vyhodnotenie (M)
1. Zapis vysledku (M)

> Wired AND

| Bit | Negacia bitu | Vyznam                                                           |
| --- | ------------ | ---------------------------------------------------------------- |
| 0   | 0            | Na zbernici su rozne zariadenia s roznou hodnotou adresneho bitu |
| 0   | 1            | Vsetky zariadenia maju na danom mieste `0`                       |
| 1   | 0            | Vsetky zariadenia maju na danom mieste `1`                       |
| 1   | 1            | Ziadne zariadenie nie je pritomne                                |

V podstate B-tree

Rychlost hladania - 75 zariadeni za sekundu

## Ethernet

IEEE 802.3

Zbernica, zdielane medium (koaxial) - historicky

- 10BASE5 - Thick Ethernet
- 10BASE2 - Thin Ethernet

Hviezda, strom (krutena dvojlinka, optika)

- 10BASE-T
- 100BASE-TX - Fast Ethernet
- 1000BASE-T - Gigabit Ethernet
- 10GBASE, 25, 50, 100, 200, ...
- 400GBASE-T, S, L, H, ...

T - twisted, X - nieco o kodovani,

Vlastnosti

- Komunikacia zalozena na prenose paketov
- Zdroj aj ciel - jedinecna 48bit MAC adresa
- Vsetky verzie - rovnaka struktura ramca
- Typ ramca - EtherType - samoidentifikujuce sa - koexistencia viac typov protokolov, aj v jednej sieti

Zdielane medium - CSMA/CD - predtym ako zacnem vysielat, pocuvam ci je ticho  
Moznost vzniku kolizii - dlzka media, pocet stanic, velkost a pocetnost paketov  
Pri kolizii sa oba pakety znicia

Hviezda, strom

- Repeater - iba opakuje
- Bridge - kontroluje hlavicku
- Switch - kontroluje cely paket

Pokrocile siete

- Redundancia - slucky - STP
- Vyvazenie zatazenia siete - SPB (shortest path bridging)
- VLAN
- Multilayer Switch
- Link Aggregation

Struktura paketu

- Preamble 7B - striedanie `1` a `0` - bitova synchronizacia
- Start of frame delimiter - `10101011` - bajtova synchronizacia
- Ethernetovy ramec
- Interpacket gap - 12B

Normalny paket - do 1500B  
Jumbo packet - zalezi od rychlosti - nad 1GBps >9000B

Eth ramec

- Source MAC 6B
- Destination MAC 6B
- Optional 802.1Q tag 4B - VLAN
- Dlzka alebo EtherType - <1500 je to dlzka, >1536 EtherType
- Data 46/42B az 1500B
- Frame check sequence - FCS 4B - 32bit CRC

Fyzicka vrstva

- Obvod PHY
- Synchronny prenos
- 10BASE-T - Manchester kod - hodiny XOR data
- 100BASE-TX - `4B5B` kod - kazda stvorica bitov sa zakoduje pomocou 5 bitov aby sa zarucila zmena
- 1000BASE-SX (opticky kabel) - `8b/10b` kod, symbol rate 1.25GBd
- 1000BASE-T (krutena dvojlinka) - 4 linky s 5-urovnovou modulaciou, symbol rate 125MBd

## USB

Universal Serial Bus  
Nahrada roznych seriovych rozhrani - RS232, LPT, Game Console

Vo vstavanych systemoch

- Komunikacia s PC - konfig, firmware, prenos dat, gateway, ...
- Pripojenie externych zariadeni - kamera, disk, klavesnica, ...

Vlastnosti

- Asynchronne
- NRZ Space kodovanie (bit stuffing) - musime zabezpecit zmeny
- Diferencialna signalizacia (nie single ended)
- Single Master
- Hviezda
- Half-duplex
- Napajanie 5V, 500mA

Verzie

- USB1.0
- USB1.1
- USB2.0
- USB3.2 Gen 1
- USB3.2 Gen 2
- USB3.2 Gen 2x2
- USB4
- USB4 2.0

Endpoints

- 0 je obojsmerny

Typy rur (pipes) pre endpoint 0

- Sprava - kratke obojsmerne spravy
- Tok (stream)
  - Izochronny - garantovana kapacita, neopakovane
  - Prerusovanci - interrupt - rychla odozva, opakovany prenos - klavesnica, mys
  - Hromadny (bulk) - zvysok prenosovej kapacity, menej dolezity prenos

Enumeracia - host potrebuje zistit kto sa pripojil

- Reset zbernice - urcenie rychlosti
- Identifikacia zariadenia - PID, VID (product, vendor id)
- Pridelenie adresy (7bit)
- Zavedenie ovladacov

Komunikacia - 3urovnove zapuzdrenie

- Paket - najmensia jednotka
- Transakcia - niekolko paketov (token, data, status)
- Prenos - niekolko transakcii (setup, data, status)
- Ramec - niekolko prenosov

Casovy multiplex -

## Sum komunikacnych rozhrani

| Rozhranie | Rychlost          | Dosah      | Sync           | Signalizacia              | Duplex | Topologia       | Master/slave          | Integrita              | Potvrdzovanie sprav       | Poradie bitov |
| --------- | ----------------- | ---------- | -------------- | ------------------------- | ------ | --------------- | --------------------- | ---------------------- | ------------------------- | ------------- |
| RS-232    | 115kbps           | 15m        | async          | single-ended              | full   | point-to-point  | nie                   | -                      | iba SW                    | LSB first     |
| CAN       | 1Mbps, 5Mbps      | 40m @1Mbps | async          | diferencialna             | half   | bus             | multi-master          | CRC, ACK, bit stuffing | ACK slot                  | MSB first     |
| LIN       | 20kbps            | 40m        | async (casove) | single-ended              | half   | bus             | 1 master, multi slave | checksum               | implicitne slave odpovede | LSB first     |
| SPI       | desiatky Mbps     | 1m         | sync           | single-ended              | full   | master + slaves | master/slave          | -                      | iba SW                    | MSB alebo LSB |
| I2C       | 100kbps - 3.4Mbps | 1m         | sync           | single-ended (open drain) | half   | bus             | master/slave          | ACK/NACK               | ACK/NACK                  | MSB first     |
| 1-Wire    | 16kbps            | 100m       | async          | single-ended              | half   | bus             | master/slave          | volitelne CRC          | presence pulse            | LSB first     |

## Bluetooth a BLE

Povodny ciel - nahrada drotoveho spojenia na kratku vzdialenost  
Firma Ericsson  
Kratky dosah, nizka spotreba, low-cost  
ISM Pasmo - 2.4 GHz  
79 kanalov, 1 MHz  
FHSS - Frequency hopping spread spectrum - 1600 hopov za sekundu

Rychlost

- Basic rate 1Mbps - GFSK (Gaussian Frequency Shift Keying)
- Enhanced Data Rate (EDR) od v2.0
  - 2Mpbs
  - 3Mpbs

Komunikacia

- Zalozena na paketoch
- Master/Slave architektura - 1 master, max. 7 slaves - piconet - novsia verzia max. 14 slaves
- Scatternet - prepojenie viacerych piconetov (master v jednej sieti, slave v inej)
- Synchronizacia mastrom - kazdych 312.5 $\micro$s - tik hodin 3.2 KHz
- Slot - 625 $\micro$s
- Par - 1250 $\micro$s - vymena paketov master-slave
- Master zacina v parnom slote, slave v neparnom
- Paket 1, 3 alebo 5 slotov
- V jednom case komunikacia len s jednym - round-robin
- $Vykon [dMb] = 10 \times log \dfrac{Vykon[mW]}{1mW}$

Bluetooth profil

- Definicia moznych aplikacii, popis psravania sa zariadenia
- Nastevania (parametrizacia) a riadenie komunikacie
- Jednoduche vytvorenie spojenia
- Obsahuje
  - Zavislost na inych formatoch (profiloch)
  - Doporuceny format uzivatelskeho rozhrania
  - Casti Bluetooth stacku pouzivane porfilom a nastavenia parametrov
- Profily
  - Advanced Audio Distribution Profile - A2DP
  - Human Interface Device Profile - HID
  - Hands-Free Profile - HFP
  - Prenos suborov, remote control, tlac, video, LAN, Mesh, proximity
  - ...

### Bluetooth Low Energy

Od BT verzie 4.0  
Malo prenasanych dat - minimalizacia pouzivania radioveho prenosu  
Nie je compatible s BT Classic - iba 40 kanalov so sirkou 2 MHz, discovery na 3 kanaloch (32 pri BT Classic)  
Volitelne sifrovanie - AES CCM, 128bit kluc  
Spatna kompatibilita  
High-speed (povinne) alebo long-range (nepovinne, Coded PHY)  
Asymetricke, vacsina prace na masterovi

---

Inzerovanie (Advertising)

UUID

### Bluetooth Mesh

Topologia many-to-many (oproti hviezde pri klasicom BT) - preposielanie sprav  
Iba software nadstavba

Vlastnosti

- Zvacseny dosah oproti BLE
- Self-healing
- Prenos sprav pomocou inzerovania a skenovania
- .

## Spolahlivy prenos dat

Zdroje chyb

- Sum
- Impulzy

Typy chyb

- Nahodne s urcitou pravdepodobnostou
- Zhluky (burst)

Spolahlivy prenos - odosielatel je informovany o uspesnom doruceni dat  
Spatna vazba

- Potvrdzovacia
  - Pozitivne potvrdenie - data boli prijate
  - Negativne potvrdenie - data neboli prijate
  - Pozitivne aj negativne potvrdzovanie
- Detekcna
- Informacna

Sposob kontroly - extra info

- Len detekcia - kontrolne sucty, parita, CRC
- Detekcia s opravou (samoopravne kody, 2D parita)

Typy potvrdzovania (bud `ACK` alebo `NAK`)

- Jednotlive
  - Po kazdom pakete
  - Druhy paket sa posle ked prvy je potvrdeny
- Priebezne (kontinualne)
  - Posiela sa furt, prelina sa
  - Necaka sa na potvrdenie
  - 2 druhy
    - Selektivne - retransmission jedneho paketu ktory sa stratil (viac efektivne)
    - S navratom - retransmission vsetkym paketov, zacinajuc tym paketom ktory sa stratli (mozno mensi zmatok)

### Detekcne kody

- Kazdy kod detekuje len urcity pocet chyb, kazdy je vhodny na nieco ine
- Detekcia **nikdy** nie je 100%
- Ochrana voci nahodnym chybam, nie umyselnym

Hammingova vzdialenost

- Pocet bitov, v ktorych sa dve slova lisia
- Pre kody: minimalna vzdialenost dvoch kodovych slov ($d$) (akoze najhorsia moznost)
  - Detekcia: $d - 1$ chyb, cize $d$ musi byt minimalne `2`
  - Oprava: $\dfrac{d - 1}{2}$, cize $d$ musi byt minimalne `3`

Cize napr. ked mame `10110` a `00111`, tak sa lisia v 2 bitoch, cize vieme detekovat

#### Opakovaci kod

- Opakovenie dat niekolko krat
- Nizky _code rate_ - pomer uzitocnych dat
- Mozem _code rate_ prisposobit vlastnostiam kanala - malo sumu = malo opakovania, vela sumu = vela opakovania
- Opakovanie dat jeden krat - detekcny kod - posielam `0` -> `00`, pride mi `01` alebo `10`, tak viem ze nieco je zle
- Opakovanie dat viac krat - samoopravny kod - posielam `0` -> `000`, pride mi `100`, tak viem ze `1` je zle

#### Sucet modulo M

Postup

1. Scitat slova (scitavaju sa postupne ako sa prijimaju data)
2. Vydelit cislom `M`
3. Kontrolny sucet = zvysok po deleni; Ina moznost = doplnok zvysku

Ked budem delit cislom `15`, tak zvysok moze byt `0` az `15`  
Ak dostanem `3`, tak doplnok (do `M` (do `15`)) je `12`

Ak ? = `0` tak sme prijali spravne (?)

Vlastnosti

- Dokaze odhalit chybu v jednom bite
- Pri 2 bitoch moze byt problem ak chyba nastane v tom istom bite, pravdepodobnost $< \frac{1}{n}$
- Nezohladnuje poradie udjov
- Nezachyti vkladanie/mazanie nul

#### Fletcher checksum

Okrem obycajneho suctu sa robi aj _sucet suctov_

| Prenasane cislo | Sucet | Sucet suctov |
| --------------- | ----- | ------------ |
| 5               | 5     | 5            |
| 6               | 11    | 16           |
| 7               | 18    | 34           |
| 8               | 26    | 60           |
| 9               | 25    | 95           |
| Sucet mod 9     | 8     | 5            |

Prenesieme aj checksum `8` aj `5`

V praxi sa deli takym cislom, aby zvysok mal co najviac moznosti - pri 8bit najcastejsie modulo 256 alebo 255 (alebo prvocislo?) - aby sa nahodou nestalo ze zly chechsum padne na take iste cislo ako dobry checksum

Rozne druhy

- Fletcher-16 (8bit sucet, 8bit sucet suctov)
- Fletcher-32 (16bit a 16bit)
- Fletcher-64 (32bit a 32bit)

#### Kod 2 z 5

V 5bit slove su 2 jednotky  
Napr. na zakodovanie desiatkovych cislic (0-9)

Hammingova vzdialenost = `2`

Moc sa pri prenose nepouziva  
Vyuzitie skor pri niektorych ciarovych kodoch

#### Parita

Pre male bloky dat (1 bajt)

Hovori o pocte jednotiek - parna alebo neparna - doplna na parny resp. neparny pocet jednotiek  
Dokaze odhalit neparny pocet chyb

Sposob generovania parnej parity

- XOR vsetkych bitov
- Sucet modulo 2
- CRC (polynom x + 1)

#### CRC

Cyclic Redundancy Check

Odhaluje zhluky chyb (bursts) do dlzky `n`  
Lahka implementacia do HW  
Analyticke vyhodnotenie ucinnosti

$\dfrac{data}{polynom} = celociselna cast + zvysok CRC$

Data povazujeme za polynom - data = koeficienty polynomu  
To iste ako delenie so zvyskom

Namiesto odcitania sa pouziva XOR

Vypocet

1. Pod data napisem polynom - napr. `1011` = $1x^3 + 0x^2 + 1x^1 + 1x^0$
2. Ak `0` tak pokracujeme, aj `1` tak `data XOR polynom`
3. Opakujeme pr vsetky datove bity
4. Ostane CRC

Treba si za data doplnit `n-1` nul, kde `n` je stupen polynomu

```txt
              CRC
data 101001 | 000
poly 1011   |
     -------------
     000101 | 000
        101 | 1
        000 | 100
```

Polynom musi mat prvu aj poslednu `1`, cize potom nemusime prenasat jednu z nich

> Polynom musi byt "prvocislo"?

Priklady generujucich polynomov

### Hash

Pouzivane pri elektronickom podpise, distribucii suborov  
Message digest  
Keyed hash - moznost ochrany pred umyselnym podvrhnutim

## Samoopravne kody

ECC (Error Correction Code), oznacovane aj ako FEC (Forward Error Correction)

- Pridanie dodatocnych informacii
- Vyuziva sa ked je narocne alebo nemozne opakovanie prenosu

Hammingova vzdialenost aspon 3 - 2 chyby detekovat alebo 1 chybu opravit

Systematicke - priamo obsahuju povodne data  
Nesystematicke - data su zakodovane

Blokovo - pracuju s blokom dat  
Konvolucne - tok bitov - doplnkove data su pridavane priebezne

Vlastnost _"vsetko alebo nic"_

- Spravna funkcnost nad urcitym prahom ruchu
- Pod prahom uplne nefunkcne

### 2D parita

Robi sa parita "po riadkoch" a zaroven "po stlpcoch", nasledne aj "parita parit"

Vdaka tomuto je Hammingova vzdialenost `4` => datekcia `3`, oprava `1`  
Vieme opravit aj viac, ak je neparny pocet chyb v jednom riadku (stlpci)

Moze byt aj `n`-dimenzionalna

### Opakovanie bitov

Kazdy bit prenesieme `n`-krat

Hammingova vzdialenost = `n`  
Ak `n = 3`, je to Hammingov kod `H(3,1)`

### Hamingove kody

`H(n,k)`

- `n` = pocet bitov kodu
- `k` = pocet bitov dat

`r = n - k` - pocet kontrolnych (paritnych) bitov

Najcastejsie $H(2^r - 1, 2^r - r - 1)$ - `H(3,1)`, `H(7,4)`, `H(15,11)`, `H(31,26)`

Dokonaly kod - najlepsi pomer $\dfrac{k}{n}$ pre vzdialenost `d=3`

Vzdy nam ostava jeden bit - mozeme spravit paritu  
SECDED (Single Error Correcting, Double Error Detecting)  
ECC Pamate

`H(7,4)`

- Datove bity $D$, Paritne bity $P$
- $D_4 D_3 D_2 P_3 D_1 P_2 P_1$
- (Prijata parita) XOR (Vypocitana parita)
- Vypocet parity
  - $P_1 = D_1 \oplus D_2 \oplus D_4$
  - $P_2 = D_1 \oplus D_3 \oplus D_4$
  - $P_3 = D_2 \oplus D_3 \oplus D_4$

### Reed-Solomon kody

Oprava 2 typov chyb

- Erasure - pozname miesto, nepozname hodnotu
- Error - nepozname nic

$2 \times e + v \leq (n-k)$

Kde

- $e$ - pocet errors
- $v$ - pocet erasure
- $n$ - celkova dlzka kodu
- $k$ - dlzka dat
- $n - k$ - minimalna vzdialenost - pridane informacie

Optimalny FEC kod

Kodovanie

- Znaky spravy = koeficienty polynomu
- Delenie generujucim polynomov
- Zvysok = RS kod

Generujuci polynom - $(x-a^0) \times (x-a^1) \times (x-a^2) \times (x-a^3) \times \dots$, napr. $a = 2$

### Hadamard-ov kod (Reed-Muller)

### Glay-ov kod

### Turbo kody

### LDPC kody

### Polar code

## Sifrovanie

Zabezpecenie dat proti

- Odpocuvaniu
- Umyselnej zmene
- Zamene identity

Dva sposoby

- Symetricke
- Asymetricke
