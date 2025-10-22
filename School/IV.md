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
