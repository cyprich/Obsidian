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
