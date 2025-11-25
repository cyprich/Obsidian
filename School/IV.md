# IV

Internet Veci

## Uvod

\4. Priemyselna revolucia

Definicie

- Technicka definicia - Ide o vzajomne a vacsinou bezdrotove prepojenie viacerych zariadeni, ktore spolu komunikuju pomocou internetu alebo vlastnej siete. Zariadenie musi komunikovat s ostatnymi zariadeniami a musi byt schopne snimania, zberu dat, obladania alebo spracovavania dat
- Priemyselna definicia - Vzajomne prepojena siet snimacov, aktuatorov a riadiacich systemov, ktore maju za ciel dosianut vyssiu efektivitu, bezpecnost alebo komfort v porovnani so stavom pred ich nasadenim

Rozdiel medzi zariadenim a vecou je taky, ze vec sa vie autonomne rozhodovat

## Hardware v IoT

Embedded system je pocitacovy system so specializovanou funkciou a je sucastou vacsieho zariadenia

Hlavnym rozdiel medzi embedded a vseobecnym systemom je v parametroch, napr.:

- Spotreba energie
- Rozmery
- Rozsah operacii, ktore sa daju vykonavat v systeme
- Naklady na vypoctovu jednotku (Hz/EUR, MB/EUR, ...)

### Snimace - senzory

Sluzia na meranie parametrov a vlastnosti fyzickeho sveta  
Napr. osvetlenie, vlkhost, teplota, CO2  
Menia neelektricky velicinu na elektricku velicinu (napatie, prud, elektricky odpor)  
Podla vystupu - analogove, cislicove

Dolezite parametre

- Pracovne podmienky - teplota - kazdy snimac ma specifikovany teplotny rozsah a presnost
- Linearita - idealny snimac by mal mat linearnu odozvu - graf merania a vystupneho napatia by mala byt priamka, nie krivka, no v skutocnosti ziaden snimac nie je dokonale linearny
- Citlivost - aku minimalnu zmenu hodnoty dokaze snimac zaznamenat. Citlivost prichadza na ukor linearity
- Doba odozvy - cas, za ktory je je snimac schopny zaznamenat zmenu. Na toto moze vplyvat vela faktorov
- Presnost - vplyv moze mat vyber hardwaru, kabelaz, relativna blizkost inych zariadeni, tienenie, uzemnenie, ... Zariadenia sa musia kalibrovat (pravidelne - rocne)
- Zivotnost - niektore snimace vydrzia dlhsie, niektore menej, ci uz z dovodu kontrukcie alebo pouzitych materialov
- Cena

Priklady snimacov - pohybovy senzor, ultrazvukovy senzor, svetelny senzor

### Ovladace - kontrolery

Arduino UNO, NANO, MEGA - mikrokontrolery
Raspberry PI - jednodoskovy pocitac

### Akcne cleny - aktuatory

### Komunikacny model

### Komponenty IoT zariadenia

### Snimace a akcne cleny

### Riadiace systemy

## Arduino

> 8-bit, 5V

Zakladne modely

- Arduino UNO
- Arduino LEONARDO
  - Derivat UNO
  - Micro USB port
  - nativne USB (bez prevodnika)
- Arduino 101
- Arduino ESPLORA

Pokrocile modely

- Arduino MEGA
- Arduino ZERO
- Arduino DUE
- Arduino MEGA ADK
- Arduino M0
- Arduino M0 PRO

Platformy vylucne pre IoT

- Arduino YUN
- Arduino ETHERNET
- Arduino TIAN
- Arduino INDUSTRIAL 101
- Arduino LEONARDO ETH
- Arduino MKR FOX 1200
- Arduino MKR GSM 1400

## ESP32

## Raspberry Pi

## Senzoricky/Senzorovy subsystem

Zakladne delenie - digitalne a analogove  
Delenie podla toho co meraju - ci meraju elektricku hodnotu a premienaju ju na neelektricku alebo naopak, alebo elektricka-elektricka a pod.

### Analog

Analogove rozhranie - hodnota snimaneho signalu je umerna velkosti vstupneho napatia alebo dobe trvania inpulzu (sirku impulzu)  
Napr. teplota (termistor), vzdialenost (echolokacia), naklon, zrychlenie  
Na vystupe generuju elektricky analogovy signal, ktory je spojity v case a hodnote

Termistor - elektricky odpor zavisi od jeho teploty  
PTC vs. NTC (Positive Temperature Coefficient vs. Negative Temperature Coefficient)

Fotorezistor - podobne ako termistor-teplo je fotorezistor-osvetlenie

Meranie vzdialenosti - ultrazvukovy senzor

### Digital

Digitalne rozhranie - hodnota snimaneho signalu je reprezentovana ciselne - signal diskretny v case a hodnote  
Digitalny vystup 0/1, Seriove rozhranie (RS232, I2C, SPI, 1 wire)

Senzor naklonu

PIR Senzor - Passive Infrared

RS232

I2C - 2 linky na vytvorenie zbernice, az desiatky senzorov a zariadeni, half duplex, 100kbps  
SCL (Serial Clock Line) - hodiny  
SDA (Serial Data Line) - data

1 wire - iba jedna komunikacna linka

### Prevod medzi nimi

A/D prevod - z analogoveho na digitalny

1. Odstranenie zloziek signalu s vyssimi frekvenciami
2. Vzorkovanie
3. Kvantovanie - linearne alebo nelinearne (logaritmicky?)
4. Kodovanie

## Akcne cleny

### LED diody

Rozne farby tvary a prevedenia  
THT, SMD  
Pripojenie na GPIO cez rezistor

Indikacne - iba take aby svietilo  
Vykonove - osvetlenie v miestnosti napr.

### Krokovy motor

### Jednosmerny motor

## Komunikacny subsystem

Drotove - SPI, I2C, 1-Wire  
Cez siet - Ethernet, WiFi, Bluetooth

## Moznosti ukladania a spracovania nameranych dat

Bud lokalne alebo na vzdialeny server/ulozisko

## Sukromie a bezpecnost

### Architektura IoT

1. Hardware (Devices)
2. Komunikacna vrstva (Edge)
3. Analyticka vrstva (Data Intelligence)
4. Aplikacna vrstva (Services)

Verifikacia dat - spravnost, uplnost, neporusenost  
Filtrovanie dat - odstranenie chybnych, neuplnych, poskodenych, podvrhnutych  
Agregacia dat - odstranovanie viacnasobnych zaznamov, statisticke predspracovanie dat, znizovanie objemu dat odosielanych dalej, uspora energie

### SWOT analyza IoT

Strengths, Weaknesses, Opportunities, Threats

### OWASP IoT

Neziskova organizacia

### Typy kybernetickych utokov

### Priebeh utoku

1. Prieskum a ziskavanie informacii
2. Testovanie (otvorene porty, ...)
3. Ziskanie pristupu
4. Udrzanie pristupu
5.

### Sifrovanie

Symetricke, asymetricke

## Zdroje napajania

### Primarne clanky

Baterie  
Urcene na jedno vybitie  
Existuju v roznych velkostiach s roznou kapacitou

Jenorazove - nenabijatelne

- ZnC - zinkovo-uhlikove - najmensia kapacita, malo napatia, nizke samovybijanie
- ZnCl - chloridovo-zinkove
- AlMn - alkalicko-manganove - idealne pre energeticky narocne pristroje
-

Alkalicke - nizsi vnutorny odpor, nizsia miera samovybijania, nevytekaju, citlive na prebitie a podbitie (nicenie clanku, zahrievanie, moznost poziaru)

### Sekunarne clanky

Akumulatory  
Mozno pouzit viac krat  
Schopnost akumulovat elektricku enrgiu opatovnym nabijanim  
Zivotnost zavisi

Viacnasobne pouzitie - nabijatelne

- Pb - Olovene - najcastejsie v autach, teraz uz nie, vysoke samovybijanie
- Bezudrzbovy oloveny akumulator
  - VRLA - gelove akumulatory, zname ako ventilom riadene olovene akumulatory
  - AGM - akumulatory s viazanym elektrolytom
  - SLA -
- NiCd - Niklovo-kadmiove
- NiMH - Niklovo-metalhydridove
- Li-Ion - litiovo-ionove
- Li-Po - litiovo-polymerove

### Linearne stabilizatory

Stabilizuju vystupne napatie, ktore musi byt nizsie? ako vstupne

### DC-DC menice

Topologie

- Nabojova pumpa
- Znizovac napatia
- Zvysovac napatia
- Invertor

## Smart City

Nema len jednu presnu definiciu  
Nie je nejaka jednotna normalizacia

Koncept smart city v sebe spaja niekolko zloziek

- Inovativne vyuzitie informacnych technologii
- Efektivnu dopravu
- Udzratelnu spotrebu enerigi
- Ciste zivotne prostredie

Smart governance

- Inteligentna sprava veci verejnych
- Znaky
  - Ucast obyvatelov na rozhodovani
  - Kvalita verejnych a socialnych sluzieb
  - Transparentne vladnutie
  - Dlhodoba strategia rozvoja mesta

Big Data

- Vela senzorov, vela dat
- Nie vsetky data su uzitocne
- Potrebujeme len vycuc z toho alebo co

Machine learning

European Innovation Partnership on Smart cities and Communities

## Inteligentne Budovy

Tiez nie je len jedna definicia

Inteligentna budova zabezpecuje produktivne a nakladovo efektivne prostredie pomocou optimalizacie 4 zkladnych prvkov

- Stavebnej konstrukcie
- Technickych zariadeni
- Sluzieb a manazmentu a ich vzajomnych vztahov

Inteligentna definicia je budova zabezpecujuca kvalitne vnutorne prostredie pri minimalnej spotrebe zdrojov a

Aktivna, pasivna a latentna inteligencia

## Priemysel 4.0

Industry 1.0 - steam - 1784  
Industry 2.0 - electrical energy - 1870  
Industry 3.0 - electrical components - 1969  
Industry 4.0 - IoT? digitalization - 2000?
