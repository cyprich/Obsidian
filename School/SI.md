# SI

Softverove Inzinierstvo

> Zrkadlo je najdolezitejsie

Hodnotenie (110 bodov spolu)

- Semester 70 - projekt 25/50, kviz 10, prezentacia 10
  - Projekt
    - prezentacia 1 - 6. tyzden, 20 bodov
    - prezentacia 2 - 12. tyzden, 30 bodov
- Skuskove 40 - test 20/40

## Uvod

Inzinierstvo = pouzitie vedeckych prinicpov pri navrhu a tvorbe strojov, stavieb, struktur, ...  
Softverove inzinierstvo = pouzitie inzinierstva pri tvorbe softwaru, zaobera sa vsetkymi aspektami tvorby softveru

Ciel

- Inziniersky pristup k tvorbe systemov
- Rozne aspekty tvorby systemov
- UML
- Praca v time
- Objektovy pristup

Zakladne prvky vyvoja softveru

- Cinnosti - procesy, ktore napomahanu zaistit, ze vysldny produkt je spravny, kompletny a zrozumitelny - konkretny navod na vykonanie cinnosti
- Metodika - sekvencia cinnosti, ktore napomahaju pri vyvoji finalneho produktu
- Nastroje (podpora)

Fazy vyvoja softveru

- Analyza
- Architektura
- Implementacia
- Testovanie
- Udrzba

## Biznis Analyza

### Studia zrealizovatelnosti

Ci sa to vobec oplati  
Analyza idealne co najlacnejsie a najrychlejsie

### Analyza domeny a identifikacia poziadaviek

Zistovanie poziadaviek,...

Problemy

- Nerealisticke poziadavky
- Nutnosti pochopenia poziadavky
- Hladanie spolocnych, nekonfiktnych casti
- Individualne zaujmy manazerov
- Zmeny poziadaviek

Specifkacia poziadaviek

- Pouzivatelske poziadavky od zakaznika
- _poziadavky od_
- Vysledok = \_

Validacia poziadaviek

### Interview

Zber faktov, nazorov, spekulacii  
Sledovat rec tela a emocie

Odporucania

- Byt pripraveny
- Neutralnost
- Robenie poznamok
- Hladat ine pohlady
- Po stretnuti spravit zapis

Otazky nepokladat stylom, aby bolo navadzane k spravnej odpovedi

## Biznis modelovanie

Spracovanie

- Pre dalsich clenov timu
- Pre zakaznika
- Defincia spolocnych pojmov
- Strukturovanie a filtrovanie
- Zrozumitelnost
- Nove otazky
- Identifikacia nekonzistentosti
- Identifikacia problemov
- Zaklad na dalsiu pracu

### Identifikacia dolezitych procesov

### Domenovy model

## Specifikacia poziadaviek

DSP - Dokument Specifikacie Poziadaviek

Sposob specifikacie poziadaviek

- Specifikacia rozhrani
  - API
  -
- Diagramy UML
- Pseudokody
  - Jazyk s abstraktnymi kontrukciami
  - Lepsie vyjadrenie vnorenych podmienok a cyklov
- Pripady pouzitia
- Formulare
  - kto, co, preco, poznamky, priorita, nejasnosti
  - vypis vstupov
  - vypis vystupov
  -
- Prirodzeny jazyk
  - Zrozumitelny pre vyvojara aj uzivatela
  - Nevyhody - nejednoznacnost, zlozite koncepsie (algoritmy) su tazko popisatelne, modularizacia, automatizacia...
  - Nutnost sa vyhybat dlhym svetiam, terminom s niekolkymi prijatelnymi vyznamami, nekonzistencii terminov, ...

### Pripady Pouzitia

Vyznam

- Vykonnostne poziadavky
- Pouzivatelske rozhranie
- Firemne pravidla
- Datove formaty
- Vstupno/vystupne formaty
- Roly pouzivatelov

Model pripadov pouzitia (Use Case Model)

- Pouzivaju sa na popis kontextu systemu a ...
- Zakladne prvky
  - Aktor (Actor) - prvok okolia modelovaneho systemu - clovek, HW, iny SW
  - Pripad pouzitia (Use Case)
    - zakladna funkcia systemu z vonkajsieho pohladu - z pohladu klineta
    - Zoznam cinnosti zvycajne definujucich interakciu medzi aktorom a systemom za ucelom dosiahnutia ciela

Identifikacia podsystemov

Pripady pouzitia

- Popisuje urcity sposob pouzitia systemu z pohladu ...
  - Vyjadruje spravanie systemu
  - Popisuje postupnost sprav medzi aktorom a systemom
  - Poskytuje aktorovi urcity vysledok (hodnotu)
- Je prostriedok na
  - Vyjadrenie poziadaviek na system
  - Ulahcenie komunikacie s klientom
  - Komunikaciu medzi vyvojarmi
  - Testovanie systemu
  - Urcovanie ceny
  - Strukturovanie systemu

Vztahy

- asociacia - ciarka - predavac a objednanie auta
- generalizacia - trojuholnikova sipka - dvaja actori medzi sebou, dva kruzky medzi sebou
- zavislost (include, extend) - jednoducha sipka ciarkovana

Scenare

- Postupnost cinnosti v komunikacii aktora so systemom
- Forma scenara
  - Najcastejsie strukturovany text
  - Moze byt aj UML diagram - sekvencny diagram, diagram spoluprace, diagram aktivit, stavovy diagram
- Nalezitosti
  - ID a nazov
  - Strucny popis
  - Aktor ktory inicializuje use case
  - A priori podmienky pre use case
  - Kroky v scenari - cinnosti aktora a systemu
  - A posteriori podmineky use case
  - Aktor ktory dostane vysledok use case

> Zevraj nemame robit scenu pre prihlasovanie a registraciu ci co lebo nam to neuznaju

Vytovrenie popisu pre kazdy pripad pouzita

Podmineky

- A pirori podmienky - vstupne podmienky, ktore musia byt splnene pred zaciatkom vykonavania use case
- A posteriori podminky - vystupne podmienky, ktore musia byt splnene po skonceni vykonavania use case

Kroky scenara - tok udalosti

- Dobry priklad
  - 1. Use case zacina, ked zakaznik zvoli "vyplnit objednavku"
  - 2. System zobrazi formular objednavky
  - 3. Zakaznik vyplni meno a priezvisko
- Zly priklad
  - 1. Su zadavane udaje o zakaznikovi
    - Nie je jasne kto zadava tie informacie
    - Kde su zadavane
    - Ake udaje

Vetvenie krokov scenara - niektore kroky mozu mat podmienku

- 1. Use case zacina, ked zakaznik oznaci tovar v nakupnom kosiku
- 2. Ak zakaznik ...

Niekedy je lepsie napisat 2 scenare - jeden pre hlavny tok a jeden pre vedlajsi tok (alternativny scenar, kde ratam so specialnymi situaciami)

Podmienky

- A priori - identifikacia hraca - hrac je prihlaseny alebo zadal nick
- A posteriori -

## Validacia poziadaviek

- Vstup - dokument specifikacie poziadaviek
- Platnost zmenenych poziadaviek
- Konzistencia
- Uplnost poziadaviek
- Kontrola realizovatelnosti
- Overitelnost
- Sledovatelnost povodu poziadavky

Detekcia a riesenie konfliktov

- Konflikt by nemali riesit vyvojari
- Rozhodnutie o konflikte by malo byt sledovatelne az ku konkretnej osobe (zastupca zadavatela) (vid sprava poziadaviek)

Metody validacie

- Preskumanie (reviews)
  - Manualna timova kontrola poziadaviek (od zakaznika po kontraktora)
  - Formy preskumania
    - Formalne preskumanie DSP - vyvojovy tim vysvetluje zakaznikovi dosledky kazdej poziadavky
    - Neformalne - diskusia o poziadavkach so zastupcami zakaznika
- Generovanie testovacich pripadov
  - Tvorba testov poziadaviek - caste odhalovanie problemov
  - Ak je tazke vytvorit test - tazka implementacia poziadavky
- Prototypovanie
  - Predvedenie spustitelneho modelu zakaznikovi - zistenie ci zodpoveda jeho poziadavkam
  - Pomocou prototypu zakaznik najlepsie pochopi spravanie sa uzivatelskeho rozhrania
- Automaticka analyza konzistencie
  - Ak su poziadavky vo forme modelu (formalna alebo strukturovana notacia) - mozna automaticka kontrola konzistencie
- Zivotny cyklus dolezity objektov

|           | Fidelity       | Cost | Use | General traits |
| --------- | -------------- | ---- | --- | -------------- |
| Wireframe | Low            |      |     |                |
| Prototype | Middle to high |      |     |                |
| Mockup    | Middle to high |      |     |                |

Sprava poziadaviek

- Proces riadenia zmien systemovych poziadaviek
- Poziadavky z hladiska vyvoja
  - Trvale
  - Nestale
- Planovanie spravy poziadaviek stanovuje
  - Sposob identifikacie poziadaviek
  - Proces zmeny poziadaviek
  - Sledovatelnost
  - Nastroje na uchovavanie informacii o poziadavkach

Nastoje na spravu poziadaviek

- Jira, GitLab, GitHub, ...

Sledovatelnost poziadaviek (traceability)

- Definuje schopnost sledovat poziadavky
- Nastroj - matica zavislosti poziadaviek

Proces zmeny poziadaviek

- Analyza problemu a specifkacia zmeny
  - Identifikacia problemu alebo navrh na zmenu poziadavky
  - Zistovanie platnosti problemu alebo zmeny
  - Vysledok - podrobnejsi navrh zmeny
- Analyza zmeny a urcenie jej ceny
  - Urcenie, aku zmenu DSP alebo dizajnu je potrebne realizovat
  - Odhad ceny zmeny alebo noveho terminu dokoncenia
  - Rozhodnutie o pokracovani v procese zmeny
- Implementacia zmeny

Ciel

- Co ma navrhovany softverovy system robit (nie ako)
- Urcit funkcie systemu
- ...
- ...
- ...

## Analyza a navrh architektury

Analyza

- Proces rozdelenia komplexeho problemu na mensie casti, za ucelom ich lepsieho pochopenia
- Zachytenie podstatnych poziadaviek a charakteristickych rysov systemu
- Ciel - vytvorit analyticky model - konceptualny model

Rozdelenie systemu na vrstvy

- Prezentacna vrstva
- **Logicka vrstva**
- Datova vrstva

Navrh

- Presna specifikacia sposobov ako to implementovat
- Zalozeny na analytickom modeli
- Zlucenie tehnickych rieseni
  - Perzistencia objektov
  - Ich distribucia
  - Architektura
  - GUI

Pravidla tvorby diagramov

- Tvoreny v domenovom jazyku
- Rozpravajte pribeh
- Perspektiva
- Rozlisujte problematiku domeny a riesenia
- Minimalizacia vztahov
- Len prirodzena dedicnost
- Tvorit model pre maximalny pocet pouzivatelov
- Co najjednoduchsi

Cinnosti

Architektonicka analyza

Rozdelenie systemu do podsystemov

### Konceptualny model

### Stereotypy

### Diagramy tried

Uz sme sa s nim stretli pri domenovom diagrame

### Typy vztahov

Asociacia - dlhsi vztah medzi instanciami  
Agregacia a kompozicia - vztah celok/cast  
Kompozicia - silnejsi typ agregacie, instancia casti len v jednom celku - ak je celok zmazany, zmazu sa aj casti

Generalizacia

> ak mam nejaku cast, tak ak ta cast je zdielana viacerymi celkami, tak v takom pripade tam nemoze byt kompozicia  
> napr. kacka moze sidlit na dvoch rybnikoch, tak v takom pripade to v ziadnom pripade nemoze byt kompozicia  
> cize ak je niekde cislo 2 alebo viac, nemoze to byt kompozicia

> ked zmazem celok (dam zosrotovat auto) tak sa zmazu aj casti (zosrotuje sa aj motor)

### Zrhnutie

###

Nemali by byt cyklicke/krizove zavislosti medzi balickami

### Analyza pripadov pouzitia

Realizacia pripadov pouzitia

- Modelovane interakcie medzi objektami
- Ciele
  - zistenie interakcii analytikcych tried
  - Zistovanie zasielanych sprav
    - Primarne operacie
    - Primarne atributy
    - Primarne vztahy
  - .

Vystup - diagram tried, komunikacny diagram, diagram nasadenia, sekvencny diagram, stavovy diagram, diagram baickov

Navrhova trieda

- _Uplna_ a dostacujuca
- Jednoducha
- Vysoko sudrzna
- Bez tesnych vazieb

#### Navrh tried

#### Navrh rozhrani

#### Navrh navrhovych realizacii pripadov pozitia

## Agilne metodiky

"Jedinou cestou ako overit spravnost navrhnuteho systemu, je co najskor ho vyvinut, predlozti ho zakaznikovi a na zaklade jeho reakcie upravit"

Naopak od tradicnych pristupov (stanovime si funkcionalitu, cas a zdroje su variabilne), pri agilnych pristupov mame stanovene cas a zdroje, funkcionalita je variabilna

Zakladne principy

- Iterativny a inkrementalny vyvoj
- Priama osobna komunikacia v time
- Stale spojenie so zakaznikom
- Rigorozne, opakovane, priebezne, automatizovane testovanie
- Vyzaduju menej formalnych a byrokratickych artefaktov
- Najsilnejsi doraz na zdrojovy kod

Preferencne tezy agilneho programovania

- Prijat a umoznit zmenu je omnoho efektivnejsie
- Je treba byt pripraveny reagovat na nepredvidane udalosti

Preferencne skupiny

| Prednost                    | Pred                            |
| --------------------------- | ------------------------------- |
| Individuality a komunikacia | Procesy a nastroje              |
| Prevadzky schopny softver   | Obsiahla a objemna dokumentacia |
| Splupraca so zakaznikom     | Uzatvaranie roznych zmluv       |
| Reakcia na zmenu            | Striktne plnenie planu          |

Zakladne tezy

- Hodnota pre zakaznika
- Zmeny su vyhodou
- Caste dodavky
- Zakaznici pracuju s timom
- Motivacia je klucova
- Vzajomna konverzacia
- Uspech posudzovany podla fungovania
- Udrzatelny vyvoj
- Perfektny navrh a riesenie
- Jednoduchost
- Kreatiita
- Ako zvysit efektivitu

### Co to je

### Extremne programovanie (XP)

Komunikacia, jednoduchost, spatna vazba odvaha, respekt

Testovanie, pisanie zdrojoveho kodu, pocuvanie, navrh

Vyhody

- Praca v sulade s instinktami
- Interaktivna
- Inkrementalna
- Priamy postup k cielu
- Bez formalit
- Podpora v IDE, CASE

Nevyhody

- Detaily jednoduche, zlozite vykonat
- Tazke zavedenie
- Vsetci clenovia timu
- Nie je vhodne pre kazdeho cloveka

### SCRUM

### TDD

Test-driven development
