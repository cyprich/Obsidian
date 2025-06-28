# AUS

## Uvod/Opakovanie

Radsej pouzivat referenciu (`&`) ako pointer (`*`) ak sa da

- Jednoduchsia syntax (netreba dereferencovat)
- Zabera menej pamate
- Je to v podstate nieco ako alias?

```c++
int x = 20;
int* ptr = &x;  // pointer
int& ref = x;  // referencia
```

## Udaje
### Primitivne typy

Primitivne typy - priamo vstavane do programovacich jazykov  
Zvycajne cele cisla, realne cisla, znak, logicky typ (true/false), referencia (nepriamy pristup k udajom)  
Su to **skalarne typy** = mozno ich porovnavat (1 < 2, false < true)  
Mozu byt **ordinalne** - vieme jednoznacne urcit predchodcu a nasledovnika (z uvedenych vsetko okrem realnych cisel)  

1B (bajt) = 8b (bit)  
1 bajt - najmensia adresovatelna jednotka - v pamati sa vsetko uklada v bajtoch

Architektury 
- LP32 - 16-bit Windows, prve IOS
- ILP32 - 32-bit Windows, Unix
- LLP32 - 64-bit Windows - my budeme pracovat s tymto 
- LP32 - 64-bit Unix, Cygwin

C++ na LLP64:
- char - 1B
- short - 2B
- int - 4B
- long - 4B
- long long - 8B

**Endianita** - poradie, v akom sa uchovavaju bajty viacbajtoveho udajoveho typu v pamati
- little-endian
	- standardne poradie na PC
	- najviac vyznamny - najvyssia adresa
- big-endian
	- sietove poradie
	- najviac vyznamny - najnizsia adresa

**Typy uchovavajuce realne cisla** - cisla s desatinnou ciarkou (float, double) - standard IEEE-754  
- float = 4B, double = 8B
- Kazde taketo cislo je v pamati rozdelene na 3 casti
	- **Znamienko** - 1bit, ci je cislo kladne (0) alebo zaporne (1)
	- **Exponent**
		- 8bit pre float, 23bit pre double
		- O kolko miest posunieme desatinnu ciarku (ale v binarnej podobe - binarna ciarka)
		- Odcitava sa este **bias** - 127 pre float, 1023 pre double - aby bolo mozne vytvorit zaporne cisla
	- **Desatinna cast**
		- 11bit pre float, 52bit pre double
		- Miesta za desatinnou (binarnou) ciarkou
		- V standarde sa predpoklada ze pred nou je cislo `1`, teda namiesto `1.2345` sa sem ulozi len `2345`
- Spojenie pomocou vzorca $cislo = (-1)^{\text{znamienko}} \times (1 + \text{desatinna cast}) \times 2^{\text{exponent} - \text{bias}}$
- Porovnavanie realnych cisel sa prudko neodporuca pomocou `a == b` resp. `a != b`, treba pocitat aj s toleranciou - `|a-b| <= epsilon`
	- `a` a `b` su realne cisla ktore porovnavame
	- `epsilon` je tolerancia, napr. `0.001`

**Kodovanie znakov**
- Kazdy znak ma celociselnu hodnotu, pomocou ktorej sa spracovava - tzv. **code point**
- Najcastejsie kodovania 
	- ASCII
		- 7bit
		- 95 vytlacitelnych znakov
		- 33 kontrolnych znakov
	- rozsirenia ASCII
		- 8bit
		- Prva polovica - `0xxxxxxx` - zhodne s ASCII
		- Druha polovica - `1xxxxxxx` - narodne abecedy, napr.
			- ISO/IEC 8859-2 (Latin-2) - stredoeuropske znaky
			- Windows-1250 - z velkej casti rovnaky s Latin-2
	- Unicode
		- 21bit - jeden znak = viac bajtov
		- Celkova kapacita `1 112 064` znakov, v hex `U+0000` az `U+0010FFFF`
		- Viacero formatov, najznamejsie 
			- UTF-8 
				- Spatne kompatibilny s ASCII
				- Reprezentovany 1-4 bajtmi
				- Bajtovo orientovane kodovanie
			- UTF-16
				- 1 alebo 2 *dvojbajty*
				- Blokovo orientovane kodovanie
			- UTF-32
				- 1 *stvorbajt*
				- Pamatovo neefektivny, nie velmi pouzivany

Princip UTF-8
- Na zaciatku sa povie, kolko bajtov sa bude pouzivat (ukoncene nulou)
	- `0` = 1 bajt, zhodne s unicode
	- `110` = 2 bajty
	- `1110` = 3 bajty
	- `11110` = 4 bajty
- Nasledne je na zaciatku kazdeho dalsieho bajtu kontrolna sekvencia `10`

![](aus-kodovanie-1.png)

Princip UTF-16
- Najprv sa zisti ci sa nieco nachadza na 5 najviac vyznamnych bitoch - indexy 16-20
	- Ak su na tychto indexoch same 0, pouzije sa jeden dvojbajt
	- Ak je tam co i len jedna 1, pouziju sa 2 dvojbajty
		- Prvy dvojbajt ma na zaciatku `110110`
		- Druhy dvojbajt ma na zaciatku `110111`

![](aus-kodovanie-2.png)

**Typy uchovavajuce logicke hodnoty**
- bool
- Uchovava len 2 hodnoty (`0` alebo `1`), ale vzdy musi byt ulozeny v jednom bajte (8bit) - najmensia adresovatelna jednotka

Uplne vs. Skratene vyhodnocovanie
- Pri operaciach `&&` (and) a `||` (or)
- Uplne = vzdy sa vyhodnotia obidve strany vyrazu
- Skratene = niekedy staci vyhodnotit iba jednu stranu a pozname vysledok
	- Ak pri `&&` je lava strana `false`, tak vysledok bude vzdy `false`
	- Ak pri `||` je lava strana `true`, tak vysledok bude vzdy `true`

#### Typ `wchar_t`  

Viacbajtovy udajovy typ  
Jeho pamatovu reprezentaciu ovplyvnuje endianita architektury (little endian vs. big endian)  
Posledna polozka pola je znak s kodom `0` - null character  

Typy implementacie
- Na Windows - 2B typ, pouziva format UTF-16  
- Na Linux - 4B typ, pouziva format UTF-32

Vo vseobecnosti neprenositelny (nekonzistentna implementacia), cize sa neoporuca pouzivat v aplikaciach, ktore maju byt spustitelne na roznych architekturach  

### Odvodene typy

Vlastne udajove typy tvorene z primitivnych typov  
Je mozne ich tvorit  
- Agregaciou - zlucenim viacerych existujucich typov do jedneho
	- Homogenna - pole 
		- Viacere objekty toho isteho typu
		- V C++ znakom `[]` za nazov (napr. `int cisla[5]`)
	- Heterogenna - zaznam - uchovavanie udajov charakterizujuce jeden objekt (napr. student - meno, rocnik, ...)
- Enumeraciou - vymenovanim vsetkych moznych hodnot, ktore moze nadobudat

Zaznam (heterogenna agregacia)
- v c++ `struct` a `class`
- padding - udaje su doplnene prazdnymi bytmi tak, aby sa vsetky udaje zacinali na adrese zhodnej s nasobkom sirky **slova procesora**

Slovo procesora 
- Zakladne mnozstvo udajov, s ktorym dokaze procesor pracovat sucasne v jednom casovom okamihu
- Jeho velkost je definovana sirkou v bitoch, v pripade 64-bit procesorov je to 64

Traling padding
- Pridany padding nie len medzi jednotlive udaje v strukture, ale aj za celu strukturu

Odporucane usporiadanie udajov od pamatovo najvacsich po pamatovo najmensie udaje

Pole znakov - retazec - string - `char[]`, posledna polozka je *"NULL character"*  

Vymenovany typ - enum - interne ako celociselny udajovy typ (je mozne definovat aky sa konkretne pouzije) 

```c++
enum DayOfWeek : short {
	Monday, 
	Tuesday, 
	...,
	Sunday
}
```

## Pamat

Rozdelena na
- Segmenty s nemennou velkostou pocas behu programu
	- Kodovy segment - samotny program
	- Inicializovany udajovy segment
		- Globalne premenne
		- Lokalne premenne
	- Neinicializovany udajovy segment - BSS = Block Started by Symbol
- Segmenty s dynamickou velkostou pocas behu programu
	- Zasobnik - zivotny cyklus riadeny zivotnym cyklom funkcie (lokalne premenne, parametre, navratove hodnoty)
	- Halda - zivotny cyklus riadeny explicitnymi volaniami

### Zasobnik - Call stack

Malicky  
Su tu lokalne premenne definovane vo funkcii, parametre funkcie, navratova adresa, navratova hodnota

Deli sa na ramce (frames)

- Novy ramec sa vytvori vzdy ked sa zavola nejaka funkcia?
- Znici sa ked sa skonci funkcia (`}`)

Stack pointer - vrchol zachobnika - tam kde zasobnik konci  
Frame pointer - adresa zaciatku ramca aktualne vykonavanej funkcie  

Default na windowse `1MB`, stack overflow - "minul" sa zasobnik

Odovzdavanie parametrov funkcii

- Hodnotou - pass by value
  - `void print(int val) {...}`
  - Hodnota parametra je skopirovana na stack, nikdy sa nemeni povodna hodnota
  - Implicitne alokuje novu pamat pre dany typ (int `4B`, class `vela B`), pozor na velke typy (class, pole)
- Adresou - pass by address
  - `void print(int* val) {...}`
  - Kopiruje sa adresa premennej
  - Implicitne alokuje novu pamat pre pointer (`8B`)
  - Treba dereferencovat `{ (*val) += 5; }`
  - Mozeme menit, kam ukazuje
  - Mozeme poslat `nullptr`
- Odkazom - pass by reference
  - `void print(int& val) {...}`
  - Kopiruje sa adresa parametra
  - Netreba dereferencovat `{ val += 5; }`
  - Implicitne alokuje novu pamat pre pointer (`8B`)
  - Da sa zabranit modifikaciam `void print (const int& val) {...}`
  - Nemozeme menit, kam ukazuje
  - Nemozeme poslat `nullptr`
  - Kedze referencia je synonymum, tak

Navratova hodnota funkcie - nikdy nevracat odkazom ani adresou lokalne premenne - po skonceni funkcie adresa nie je platna

Prudko nepouzivat `mutable` a `const cast`

### Halda - Heap

Velka kapacita, (skoro) vsetka ostatna pamat

Vznik - `new`, `malloc`, `calloc`, `realloc`   
Zanik - `delete`, `free`  

`new` pre objektove typy vola aj konstruktor, vrati pointer do haldy, kde objekt vznikol  
`delete` pre objektove typy vola aj destruktor  

```c++
Osoba o = new Osoba();
delete o;
```

Dangling pointer

- Visiaci ukazovatel
- 2 pointre ukazuju na ten isty objekt, jeden spravi delete objektu, druhy ukazuje na neexistujuci objekt = je dangling pointer
- Riesi sa pomocou `nullptr`

Memory leak

- Aby sa dalo pristupit k pamati v halde, musi v zasobniku existovat premenna, pomocou ktorej sa da dohladat
- Ked prideme o alokovanu adresu v pamati (nahradime ju novou), nemozeme k povodnej pamati pristupovat a iba zabera miesto = memory leak
- V inych jazykoch to riesi garbage collector
- Tu na mozu pomoct Smart pointers - unique pointers, ...

### Spravca pamate

- Zodpovedny za pridelovanie a vratenie pamate
- Moze spravovat operacnu alebo externu pamat
- Kompaktna (suvisla) pamat - za sebou (bezprostredne) iduce bajty - mozu sa zgrupovat do blokov pamate
- Nekompaktna (nesuvisla) pamat - bloky su "rozhadzane" po pamati
- Udrziava informaciu o polohe a velkosti volnych segmentov v pamati

Pridelenie pamate
- First-fit - vyberie prvy vhodny (dostatocne velky) segment, rychlejsie
- Best-fit - vyberie najvhodnejsi ((priblizne) rovnaka velkost) segment, menej fragmentacie, pomalsi
- Worst-fit - udrziava zoznam volnych segmentov zoradeny od najvacsieho, pouzijem prve (najvacsie), najrychlejsie, velka fragmentacia 

Spravca operacnej pamate, spravca suvislej operacnej pamate, spravca suvislej externej pamate

Inicializacia pamate
- Pri neobjektovych typoch
	- Prideli sa pamat, moze byt inicializovana ("vygumovana") alebo neinicializovana (nahodne hodnoty)
- Pri objektovych typoch
	- Alokuje pamat + vola konstruktor
	- Pri destrukcii najprv zavola destruktor a potom uvolni pamat
	- Moze sa tu nachadzat bud objekt samotny alebo jeho adresa
	- Pozor na memory leaks a dangling pointers  

Operacie spravcu pamate
- zmen velkost pamate
- skopiruj pamat
- premiestni pamat
- ...

Spravca kompaktnej pamate - vypocitaj adresu  
$A(i) = A_{\text{baza}} + (i \times d)$   
> $A_{\text{baza}}$ = bazova adresa (baza)  
> $d$ = velkost jedneho bloku pamate (|`TypBloku`|)  

Spravca kompaktnej pamate - vypocitaj poradie  
$I(blok) = \dfrac{A_{\text{baza}} - A_{\text{baza}}}{d}$   
> $A_{\text{blok}}$ = adresa bloku pamate  

#### Expanzna strategia

- konstantna - $m + 10$
- linearna - $m * 2$
- geometricka - $m ^2$

## APT, APS, AUT, AUS, SP

### APT - Abstraktny Pamatovy Typ

Urcuje
- Ako udajova struktura organizuje bloky v pamati
- Operacie typicke pre dane pamatove usporiadanie
- Poziadavky na bloky pamate

Implementacne je typicky rozhranie (interface)  
Bloky pamate je mozne specifikovat s vyuzitim generik, sablon, konceptov, ...  
Ten isty APT je mozne realizovat viacerymi sposobmi s odlisnou organizaciou blokov pamate - vyber najvhodnejsej realizacie pre konkretnu situaciu  

Spravuje bloky pamate, pricom kazdy ma vztahovu a udajovu cast  
Je realizovany APS, pri implicitnych je vztahova cast `= 0B`, pri explicitnych `> 0B`  

Pozname tieto: 
- **Sekvencia** - blok pamate ma max. `1` predchodcu a max. `1` nasledovnika - **vztah 1:1**  
- **Hierarchia** - blok pamate ma max. `1` predchodcu (otec) a `N` nasledovnikov (synovia) - **vztah 1:N**  
- **Siet** - blok pamate ma vztah k minimalne `N-1` prvkom, potencialne teda kazdy s kazd;ym - **vztah N:N**  

Viacej v [Abstraktny pamatovy typ (APT)](#Abstraktny%20pamatovy%20typ%20(APT))  

### SP - Spravca Pamate  

Vsetky pamatove struktury, ktore ukladaju bloky pamate do rovnakeho typu pamate, budu pozadovat pri pridelovani pamate specificke spravanie - definujeme spravcu pamate  

SP urcuje sposob pridelovania a uvolnovania pamate  
Implementacne je typicky rozhranie (interface)  
Nerozlisuje bloky pamate  
Rovnakeho SP je mozne vyuzivat pre realizaciu roznych APT  

Pozname tychto:
- **Spravca operacnej pamate** - operacna pamat je alokovana pomocou zvolenej strategie (first fit, best fit, worst fit), uvolnit je mozne akykolvek blok, dochadza k fragmentacii  
- **Spravca suvislej operacnej pamate** - operacna pamat je postupne alogovana, uvolnit je mozne naposledy alokovany blok, nedochadza k fragmentacii  
- **Spravca suvislej externej pamate** - to iste co predtym len s externou pamatou  

Viacej v [Spravca pamate](#Spravca%20pamate)  

### APS - Abstraktna Pamatova Struktura  

Je realizacia APT aj SP  
APT definuje ake su vztahy medzi udajmi  
SP definuje v akej pamati su ulozene  

Ani APT ani SP nespecifikuju ani neobmedzuju bloky pamate  
Znalost o konkretnych blokoch pamate, ktore spracuva ma APS  

Viacej v [Abstraktna pamatova struktura (APS)](#Abstraktna%20pamatova%20struktura%20(APS))  

### AUT  

AUT Zoznam  
AUT Pole  

AUT Strom

AUT Zasobnik  
AUT Front  

AUT Prioritny front  

AUT Tabulka  
### AUS  

AUS Vseobecny Zoznam  
AUS Jednorozmerne Pole, Viacrozmerne Pole  

AUS Vseobecny Strom  

AUS Implicitny zasobnik, Explicitny zasobnik (LIFO)  
AUS Explicitny front, Implicitny front *s obmedzenou kapacitou* (FIFO)  

AUS Utriedeny/neutriedeny sekvencny prioritny front  
**AUMS** Dvojzoznam (kratka utriedena + dlha neutriedena sekvencia)  
AUS Lavostranna halda

AUS Utriedena implicitna sekvencna tabulka  
AUS Neutriedena implicitna/explicitna sekvencna tabulka  
**AUMS** Tabulka s rozptylenymi zaznamami (Hashtable)  
AUS Binarny Vyhladavaci Strom  
AUS Treap, Cerveno-cierny strom, AVL strom, Splay strom  

## Abstraktne pamatove struktury (APS)

### Logicke usporiadanie udajovych struktur

> Sme si sure ze *udajovych* struktur?  

![obrazok](../others/images/aus_logicke_usporiadanie_stuktur.png)

- Sekvencia (asi pole) - 1 predchodca, 1 nasledovnik - `1:1`
- Hierarchia (vyzera ako strom) - 1 predchodca, n nasledovnickov `1:n`
- Siet (total mess) - m predchodcov, n nasledovnikov - `m:n`

Udajova struktura pozostava z 2 typov blokov
- Riadiaci blok - objekt, reprezentuje samotnu udajovu strukturu, su tu ulozene jej vlastnosti, spravidla 1 
- Blok pamate - samostatne udaje spravovane strukturou, spravidla mnoho

### Abstraktny pamatovy typ (APT)

Typicky rozhranie (interface)  
Urcuje, ako su organizovane bloky v pamati (sekvencne, hierarchicky, siet)  
Urcuje operacie typicke pre dane pamatove usporiadanie  

Poziadavky na APT 
- Typicky rozhranie
- Bloky pamate je mozne specifikovat pouzitim generik, sablon, konceptov, ...
- Ten isty APT je mozne realizovat viacerymi sposobmi s odlisnou organizaciou blokov pamate, cim je mozne vybrat najvhodnejsiu realizaciou pre konkretnu situaciu 

Suvisla pamat, nesuvisla pamat  

Spravca pamate  
- Urcuje sposob pridelovania a uvolnovania pamate  
- Typicky rozhranie
- Nerozlisuje bloky pamate
- Rovnakeho spravcu pamate je mozne vyuzivat na realizaciu roznych APT

### Abstraktna pamatova struktura (APS)

Kombinuje APT a spravcu pamate  
APT - ake su vztahy medzi udajmi  
SP - v akej pamati su udaje ulozene  
Ani APT ani SP nespecifikuju ani neobmedzuju bloky pamate  


Kombinatoricka explozia
- Sekvencia
  - Implicitna sekvencia + Spravca suvislej pamate
  - Obojstranne zretazena sekvencia + Spravca suvislej pamate
  - Obojstranne zretazena sekvencia + Spravca nesuvislej pamate
  - `...`
- Hierarchia
  - `...`
- Takto sa bude zvacsovat pridanim dalsieho SP alebo APT

Problem - strasne vela kombinacii pri dedicnosti  
Riesenie - kompozicia namiesto dedicnosti
- Basically hovori ze APS sa sklada z takehoto APT a takehoto SP  

Implicitne (v *suvislej* pamati - za sebou - compact memory manager) a explicitne (v *nesuvislej* pamati - rozhadzane)

APT spravuje bloky pamate, pricom kazdy blok ma udajovu a vztahovu cast  
- Udajova cast = uzitocne informacie/udaje, ktore chceme uchovavat, z pohladu struktury nedolezite
- Vztahova cast = zabezpecuju pristup k dalsim blokom, dolezite pre strukturu, tuto cast je potrebne minimalizovat

Definovanie udajovej casti
- Predok udajov (dedicnost) - atribut je typu `Udaj`, to co chceme uchovavat bude dedit od tohto, potrebne pretypovat
- Rozhranie - atribut je interface `Ulozitelny`, to co chceme ukladat musi realizovat toto rozhranie, potrebne pretypovat
- Sablona (generikum) - typ `T`, nekladie ziadne obmedzenia, netreba pretypovat, najvhodnejsie pre toto pouzitie

Definovanie vztahovej casti 
- Vymenovanim v bloku pamate
	- Pocet vztahov musi byt dopredu znamy
	- Po jednom, alebo pomocu pola
	- Vztahy su ulozene v kompaktnej pamati
	- Vhodne pre operacnu aj externu pamat
- Ulozenim v inej APS (typicky sekvencnej)
	- V bloku pamate je uchovana referencia na APS
	- Vhodne iba pre operacnu pamat
	- Nie je garantovana suvislost pamate (zavisi od zvolenej APS)
	- Univerzalnejsie

Shallow copy (skopiruju sa iba pointre, 2 veci ukazuju na to iste miesto) vs. Deep copy (skopiruju sa samotne udaje, 2 veci su nezavisle od seba)

#### Implicitne APS

Bloky pamate nemaju vztahovu cast, pretoze vztahy vyplyvaju (implicitne) z niecoho ineho a daju sa vypocitat matematickymi operaciami  
Musia byt ulozene v kompaktnej pamati - vyhradne bezprostredne za sebou

Nahodne struktury - priamy pristup k lubovolnemu prvku struktury

#### Explicitne APS

Bloky pamate maju vztahovu cast s explicitne vyjadrenymi vztahmi  

Moze byt ulozene v suvislej aj nesuvislej pamati
- Suvisla - referencia moze byt cislo, prazdna referencia typicky `-1`, spravca kompaktnej pamate
- Nesuvisla - referencia musi byt adresa (pointer), prazdna referencia `nullptr`, spravca pamate (nie kompaktnej)

### Iteratory

_"Iterator je prst"_  
Ukazuje na nejaku poziciu v strukture a moze sa pohybovat na dalsie miesto  
Zapuzdruje prechod strukturou a zaroven efektivne spristupnuje prvky (bez odhalenia vnutornej implementacie)
Musi byt fess rychly - zlozitost `O(1)` (skoro vzdy)

Vyhody
- Umoznuje spristupnovat prvky bez odhalenia implementacnych detailov - pre volajuceho sa to stava transparentne
- Je mozne efektivne vykonavat viac prehliadok naraz (bez toho aby jedna skoncila) 
- Univerzalne algoritmy, ktore vyuzivaju iteratory, mozu byt pouzite na roznych strukturach bez zmeny kodu
- Jednotne rozhranie na prechod struktur
- Zlozitost vsetkych operacii zvacsa `O(1)`

Nevyhody
- Iterator nesmie menit strukturu, ktorou prechadza 
- Iterator je objekt = zvysenie pamatovych a vypoctovych zlozitosti (velkost iteratora, vytvorenie a zrusenie iteratora)

Typy

- Standardny
- Dopredny - viem citat od niekam po niekam
- Obojsmerny - viem posuvat aj dozadu
- S nahodnym pristupom - posun sa o niekolko

Operacie `vpred`, `spristupni`, `ma dalsi` pre vsetky iteratory

Podla ulozenia APS
- Iteratory kompaktnej pamate - uchovavaju referenciu na APS a index bloku pamate
- Iteratory nekompaktnej pamate - uchovavaju referenciu na blok pamate

APT rozsirime o *prvy iterator* a *posledny iterator*  

Prehliadky
- Sekvencne spracovanie vsetkych blokov pamate
- Ziskanie prveho bloku pamate s danou vlastnostou 
- Ziskanie poradia daneho bloku pamate s danou vlastnostou

Iteratory umoznuju vytvorit vseobecne algoritmu na tieto prehliadky, bez ohladu na na vzajomne vztahy alebo interne usporiadanie  
### Sekvencia

Linearny abstraktny pamatovy typ (LAPT)  
Jeden predchodca, jeden nasledovnik  
Kazdy blok pamate ma urcene svoje poradie - index  
**Prvok sekvencie** - blok pamate spolu s indexom  

Ak je navzajom previazany prvy a posledny prvok, tak hovorime o cyklickej sekvencii  
#### Prehliadky 

Mozne v priamom aj spatnom poradi  
Implementacne sa nelisi prehliadka implicitnej a explicitnej sekvencie  

Pozor na nekonecny cyklus pri cyklickej sekvencii

#### Implicitna sekvencia  

Vztah pre ziskanie predchodcu/nasledovnika 
- $\text{nasledovnik}(i) = i + 1$
- $\text{predchodca}(i) = i - 1$

Je vhodne zapuzdrit ich pre cyklicku sekvenciu, kde je potrebne riesit zacyklenie

Selektory - operacia spristupni - vyuziva operaciu daj blok pamate od spravcu kompaktnej pamate  
Modifikatory - vkladanie, mazanie 

##### Iterator

Je mozne implementovat ako iterator s nahodnym pristupom  
Zalozene na iteratore kompaktnej pamate ktory ma 2 atributy - referenciu na strukturu (IS) a referenciu na aktualny prvok (index)  
Zlozitosti vsetkych operacii su `O(1)`  

#### Explicitna sekvencia

Riadiaci blok typicky uchovava referencie na prvy a posledny prvok  
Ak blok pamate obsahuje iba referenciu na nasledujuci prvok - jednostranne zretazena explicitna sekvencia
Ak blok pamate obsahuje referenciu na nasledujuci aj predchadzajuci prvok - obojstranne zretazena explicitna sekvencia

Zakladne operacie - priradenie, vymazanie, porovnanie, vypocet indexu
Pomocne operacie `spojBloky` a `odpojBlok`

Selektory
- Spristupnenie prveho prvku - vzdy jednoduche - mame nan vzdy ulozenu referenciu  
- Spristupnenie *nasledovneho* prvku - vzdy jednoduche - mame nan ulozenu referenciu v aktualnom prvku

Modifikatory - vkladanie, mazanie
Iteratory  

### Hierarchia

Stromova struktura  
#### Terminologia

Kazdy blok pamate sa oznacuje ako **vrchol**

Pre lubovolne 2 vrcholy mozeme definovat ich vztah - **predok**, **potomok**  
Priamy predok = **otec**, priamy potomok = **syn**  

**Koren** - nema otca - uplny hore, iba synovia, kazda hierarchia *max. 1 koren*  
**List** - nema syna - posledny vrchol  
**Vnutorny vrchol** - aj otec aj syn(ovia) 

**Stupen** = pocet synov  
**Uroven** = ako daleko od korena som (koren = 0, jeho synovia = 1, ...)  
**Hlbka** = najvacsia uroven, uroven "najnizsieho" listu    
**Mohutnost** = pocet vrcholov hierachie

| Hierarchia                | Vztahy                    |
| ------------------------- | ------------------------- |
| ![](aus-hierarchia-2.png) | ![](aus-hierarchia-1.png) |
| ![](aus-hierarchia-3.png) | ![](aus-hierarchia-4.png) |
| ![](aus-hierarchia-5.png) | ![](aus-hierarchia-6.png) |

Podhierarchie - kazdy vrchol mozeme povazovat za *koren* vlastnej hierarchie   

#### Klasifikacia

K-cestne
- Obmedzeny pocet synov
- Binarne - $K = 2$ - vrchol moze mat max 2 synov
- Trojcestne - $K = 3$
- Stvorcestne - $K = 4$
- Atd atd...
- Dalej sa delia na usporiadane a neusporiadane (iba K-cestne)
	- Usporiadane 
		- Synovia tvoria usporiadanu mnozinu
		- Synov je mozne pomenovat nejakou vlastnostou - najstarsi najmladsi, lavy pravy, prvy druhy treti, ...
	- Neusporiadane
- Implementacne - mozeme zaviest rozhranie, ktore specifikuje `K`

Viaccestne hierarchie
- Pocet synov vrcholu nie je obmedzeny, moze byt lubovolny pocet
- V podstate $K = \infty$
- Napr. suborovy system

#### Vlastnosti

Vyvazena hierarchia - vsetky listy v hierarchii lezia v *takmer* v rovnakej hlbke

Pre K-cestne

- Kompletnost - zaplname hierarchiu postupne - nevytvarame novu uroven, ak este momentalna uroven nie je uplne zaplnena
- Plnost - kazby vrchol ma prave `0` alebo `n` synov
- Perfektnost - je kompletne zaplnena, vsetky listy su na jednej urovni, taky pekny trojuholnik

![](aus-hierarchia-7.png)

#### Prehliadky

Je mozne prehliadat do hlbky a do sirky

- Prehliadka do hlbky
	- Prehliadka v priamom poradi (**preorder**) - najprv spracujem seba, potom synov - basically prehladavame od korena smerom dole k listom
	- Prehliadka v spatnom poradi (**postorder**) - najprv spracujem synov, potom seba - basically prehladavame od listov smerom hore ku korenu
	- *Specialny pripad* - prehliadka vo vnutornom poradi (**inorder**)
		- Iba v Binarnej hierarchii
	    - Ak mam laveho syna tak ho spracujem, potom spracujem seba, potom praveho syna
	    - V konecnom dosledku spracujem celu mapu zlava doprava
	- Max. pocet ramcov (rekurzia) = hlbka
- Prehliadka do sirky (**level order**) - po jednotlivych urovniach - nie je rekurzivna (max. 1 ramec)

#### Dotazy

`jeKoren`, `jNtySyn`, `maNtehoSyna` - univerzalna implementacia  
`jeList` - zlozitost sa bude zhodovat s neskor uvedenou operaciou stupen   
`uroven` - basically vypocet indexu vrcholu v pomyselnej explicitnej sekvencii (v smere syn -> otec)  
`mohutnost` - pomocou lubovolnej prehliadky, je velmi neefektivna, nepouzivat ak ozaj netreba  

#### Implicitna hierarchia

Musi byt v kompaktnej pamati  
Mozne len pri K-cestnych hierarchiach  

![](aus-hierarchia-8.png)
##### Vypocet vztahov 

$\text{syn}(i, p) = K \times i + p$  
$\text{otec}(i) = (i - 1) div K$  
$\text{uroven}(i) = \lfloor log_K ((K - 1) \times i + 1) \rfloor$

> $K$ = druh hierarchie
> $i$ = index syna/otca/vrcholu v suvislej pamati
> p = poradie syna u svojho otca ($p \in <1 ; K>$)
> $div$ = basically delenie a zaokruhlenie nadol

Intuicia vypoctu
- Syn
	- Synovia su pozgrupovani po blokoch o velkosti `K` (K-cestna hierarchia, kazdy otec ma najviac K synov)
	- Vieme, ze za otcom na indexe `i` nasleduje jeho `K` synov
	- Staci uz len pripocitat `p` - prvy syn `+1`, druhy syn `+2`, ...
	- Napr. pre 3-cestnu hierarchiu
		- Prvy syn korena - $\text{syn}(0, 1) = 3 \times 0 + 1 = 1$  
		- Druhy syn vrcholu na indexe `1` = $\text{syn}(1, 2) = 3 \times 1 + 2 = 3 + 2 = 5$
		- Treti syn vrcholu na indexe `5` = $\text{syn}(5, 3) = 3 \times 5 + 3 = 15 + 3 = 18$
- Otec
	- Synovia su pozgrupovani po blokov o velkosti `K`
	- Zoberieme index vrcholu, odcitame `1` (pretoze indexujeme od `0`, nie od `1`) 
	- Vydelime `K` a zaokruhlime nadol  
	- Basically opacna operacia k vypoctu syna 
	- Napr. pre 3-cestnu hierarchiu
		- Otec vrcholu na indexe `7` - $\text{otec}(7) = (7 - 1)  div  3 = 6  div  3 = 2$
		- Otec vrcholu na indexe `32` - $otec(32) = (32 - 1)  div  3 = 31  div  3 = \lfloor 10.\overline{3} \rfloor = 10$
- Uroven
	- Uroven rastie logaritmicky

#### Explicitna hierarchia

Riadiaci blok typicky uchovava referenciu na koren hierarchie  
Bloky typicky uchovavaju referenciu na otca a na synov  

## Abstraktne udajove struktury (AUS)

Abstraktnu udajovy typ - AUT - podobne ako pamatovy, ale uz s konkretnymi datami/hodnotami?  

Ak AUS vyuzivaju implicitnu APS
- Nizsie pamatove naroky
- Efektivny pristup k prvkom
- Pomale modifikacie

Ak AUS vyuzivaju explicitnu APS
- Vyssie pamatove naroky
- Neefektivny pristup k prvkom
- Efektivne modifikacie

### Rozdiel oproti APS

APS pracuju s blokmi pamate
- `<TypBloku>`
- Je podstatne, co je v udajovej casti bloku pamate
- APT pre univerzalnost nema sablonovy paramter

AUS pracuju s udajmi 
- `<T>` - typ udajov
- Definuje udajovu cast bloku pamate
- AUT pre univerzalnost nema sablonovy parameter

![](aus-aps-vs-aus.png)

Kazda AUS musi kontrolovat vstupy (platnost indexu), moze zefektivnit operacie `prirad` a `porovnaj` (porovnanim `self` a vstupneho parametra)  

Ak AUS nie je multistruktura, tak musi riadi koniec zivotneho cyklu APS, nemusi sa zaoberat implementaciou zakladnych operacii (su implementovane univerzalne v APS)  

Ak AUS je multistruktura tak riadi cely zivotny proces APS z ktorych sa sklada  

AUS musi byt kompatibilna s APS (bloky pamate)  
APS musi byt kompatibilna s typom udajov AUS (sablonove parametre musia byt kompatibilne)  

### Prehliadky

Nie vsetko podporuje iterovanie - nie ze by sa nedalo, ale nedavalo by zmysel  
Iteratory sa mozu zdedit okrem metody `spristupni()`, z ktorej musime vratit len data namiesto bloku pamate  
Iteratory multistruktur je potrebne navrhnut na mieru danej multistruktury  

### Konkretne AUT a AUS

- AUT Zoznam
	- Moze byt napr ArrayList (ArrayList v jednom jazyku nemusi byt to iste ako ArrayList v druhom jazyku)
	- Pristup na zaklade indexu
	- Specialny typ - cyklicky zoznam - prvy a posledny prvok su prepojene
	- Podporuje iterovanie
	- Je to iba rozhranie, koncept, teoria, definuje ocakavane spravanie zoznamu
- AUS Vseobecny zoznam
	- Podobne ako Zoznam
	- Zoznamy su sekvencie
	- Prakticka implementacia Zoznamu spolu so sekvenciou
	- `VseobecnyZoznam<T, TypSekvencie>`
	- `ImplicitnyZoznam`, `JednostranneZretazenyZoznam`, `ObojstranneZretazenyZoznam`, + Cyklicka verzia ku kazdemu
- AUT Strom, AUS Vseobecny strom
	- Obalka hierarchie
	- AUT je len teoria, AUS je prakticka realizacia
	- `VseobecnyStrom<T, TypHierarchie>`

Rozdiel je v urovni abstrakcii  
AUT (interface) definuje ake operacie mozeme vykonavat - `spristupni()`, ...  
AUS (implementacia interface) uz konkretne vyberie nejaku sekvenciu (IS, SLS), a vola operacie nad touto sekvenciou  

### Polia

AUT Pole  
AUS Jednorozmerne pole, AUS Dvojrozmerne pole, AUS Viacrozmerne pole  

Fixna velkost po dobu celeho zivotneho cyklu, mozeme len vyberat/prepisovat prvky (nie vkladat/odoberat)  
Pristup pomocou indexu, moze byt 2-zlozkovy, 3-zlozkovy, K-zlozkovy
Pri vzniku musime naplnit - definovat hodnotu prvkom (kostruktor pri objektoch)  

Implementovane pomocou APT Implicitna Sekvencia

Regularne (plne) vs. neregularne (chybaju prvky, vnorene pole ma menej elementov)

![](aus-pole-1.png)

![](aus-pole-2.png)

#### Regularne

Mapovanie po riadkoch, po stlpcoch

Lexikograficke poradie  
Kolexikograficke poradie

##### Kompaktne

Casova zlozitost O(1) pri jednorozmernych  
Pri viacrozmernych O(K^2) kde K je pocet rozmerov  
Ked upravime vzorec, tak dostaneme zlozitost O(K), ale ten nechceme implementovat  
"Vzdy je to rozdiel\*nieco + rozdiel\*nieco + rozdiel\*nieco + rozdiel\*1"  
**Informacny vektor** - predpocitame si nieco co sa nemeni  
Informacny vektor je ze (N3xN2, N3, 1) = (15, 3, 1)  
Potom spravime dot product informacny vektor a (index - baza)

**!!! TOTO SA BUDU PYTAT NA SKUSKE !!!**

##### Nekompaktne

Udajova multistruktura - struktura z viac struktur  
Pole poli - ako v Jave  
Vytvorim pole, do neho pole, do neho pole,... vela for loopov v sebe  
Nevyhoda - ked chceme pristupovat musime robit vela "jumpov" pri viacrozmernych  
Treba robit **deep copy**

> kompilovane jazyky - variaticka sablona - nieco co nechceme  
> interpretovane jazyky - dalo by sa celkom lahko?

Pristupovanie k miestu v pamati je ovela rychlejsie ako nasobenie  
Netreba alokovat suvisle miesto v pamati, ale zabera kusok viac pamate celkovo

Mozeme si ich kombinovat - nekompaktne jednorozmerne pole ktore obsahuje kompaktne trojrozmerne polia

#### Dimenzia pola

Interval indexov prvkov pola, typicky $<0; n>$, ale v niektorych jazykoch je mozne definovat inak (Pascal, Fortran)  
Interval moze by definovany najmensim pripustnym indexom a poctom poloziek  
Najmensi pripustny index - v tomto pripade `0` - **bazicky index** alebo skratene **baza**

Ak mame pole s bazou $b$ a poctom prvkov $N_b$, tak pripustne indexy su z rozsahu ${b \dots b + N_b - 1}$  
Najvyssi platny index oznacime $c$, teda $c = b + N_b - 1$  
Dvojicu cizel $b$ a $N_b$ budeme oznacovat ako dimenzia pola, teda $D = (b, N_b)$  

Sekvencia je indexovana od `0`, pole je indexovane od `b`, takze treba **mapovaciu funkciu** - $\text{pole}[i] = \text{sekvencia}[i - b]$  

#### Viacrozmerne polia

Viacrozmerne polia maju pristup pomocou **viaczlozkoveho indexu**  
Dimenzia ma teda tiez viac zloziek, je to usporiadana dvojica/trojica/K-tica  

$(D_1, D_2) = \left( (b_1, N_{b_1}), (b_2, N_{b_2})) \right)$  
$(D_1, D_2, \dots, D_K) = \left( (b_1, N_{b_1}), (b_2, N_{b_2}), \dots, (b_K, N_{b_K}) ) \right)$  

V kompaktnej pamati ako jedna implicitna sekvencia, jej velkost = celkovy pocet prvkov  
V nekompaktnej pamati ako "pole poli", resp. "pole poli poli ..." pri vyssich dimenziach az po jednorozmerne pole  

Mapovanie po riadkoch (vs. menej pouzivane mapovanie po stlpcoch) pri dvojrozmernych regularnych poliach

![](aus-mapovanie-1.png)

##### Kompaktne viacrozmerne regularne polia (vseobecne)  

> $K$ = pocet dimenzii
> $j$-ta zlozka ma pripustne hodnoty z rozsahu ${b_j \dots c_j}$
> $N_j$ = pocet moznych hodnot ktore moze tato zlozka nadobudat, $N_j = (N_j - B_j + 1)$, pre $j \in {1, 2, \dots, K}$  

Mapovanie po riadkoch  
$\text{pole}[i_1][i_2] \dots [i_K] = \text{sekvencia} \left[ \displaystyle\sum_{j=1}^{K} \left( (i_j - b_j) \displaystyle\prod_{l=j+1}^{K} (c_l - b_l + 1) \right) \right] = \text{sekvencia} \left[ \displaystyle\sum_{j=1}^{K} \left( (i_j - b_j) \displaystyle\prod_{l=j+1}^{K} N_l \right) \right]$ 

Problem s vykonom, pretoze vzdy je treba vykonat
- `K` krat operaciu odcitania
- `K-1` krat operaciu scitania
- `K * (K-1) / 2` krat operaciu nasobenia

Samotna neupravena mapovacia funkcia ma teda zlozitost `O(K^2)`  

**Rekurentna mapovacia funkcia**

$\text{pole}[i_1][i_2] \dots [i_K]  = \text{sekvencia} \left[ \displaystyle\sum_{K}^{j=1} \left( (i_j - b_j) \displaystyle\prod_{l=j+1}^{K} N_l \right) \right] = \text{sekvencia} \left[ \left( \dots \left( (i_1 - b_1) N_2 + (i_2 - b_2) \right) N_3 + \dots + (i_{K - 1} - b_{K - 1}) \right) N_K + (i_K - b_K) \right]$ 

![](aus-mapovanie-2.png)

Touto upravou bude vzdy potrebne vykonat  
- `K`  krat operaciu odcitania
- `K - 1` krat opearciu odcitania
- len `K-1` krat operaciu nasobenia  

Zlozitost upravenej rekurentnej mapovacej funkcie but teda iba `O(K)`

##### Informacny vektor  

Mozeme si predpocitat jednotlive suciny v mapovacich funkciach, pretoze sa nemenia   
Dostaneme $K$ sucinov, ktore oznacime ako $v_j$, pricom $j \in {1, 2, \dots, K}$  
- Po riadkoch $v_j = \displaystyle\prod_{l = j + 1}^{K} N_l$
- Po stlpcoch $v_j = \displaystyle\prod_{l = 1}^{j - 1} N_l$  

Ak tieto indexy zoskupime podla rastuceho indexu do usporiadanej K-tice, dostaneme pomocnu strukturu nazyvanu informacny vektor  

Mapovacia funkcia potom nadobuda tvar  $\text{pole}[i_1][i_2] \dots [i_K] = \text{sekvencia} \left[ \displaystyle \sum_{j=1}^{K} (i_j - b_j) v_j \right]$  
S vyuzitim skalarneho sucinu ($\odot$) dostaneme $\text{pole}[i_1][i_2] \dots [i_K] = \text{sekvencia} \left[ (I - B) \odot V \right]$, pricom $I = (i_1, i_2, \dots, i_k); B = (b_1, b_2, \dots, b_K)$  

![](aus-mapovanie-3.png)


#### Kompaktne vs. Nekompaktne polia

Zlozitost (casove naroky) 
- Teoreticky rovnaka - `O(K)`  
	- V kompaktnej pamati - mapovacia funkcia ktora vykonava `O(K)` jednoduchych matematicky operacii  
	- V nekompaktnej pamati - pristup do `K` jednorozmernych poli, pricom kazdy pristup je `O(1)` - skok v operacnej pamati
- Prakticky moze byt velky rozdiel
	- V kompaktnej pamati - trivialna matematicka operacia je velmi rychla
	- V nekompaktnej pamati - **skok v operacnej pamati je vyrazne casovo narocnejsi = pomalsie**, aj ked teoreticky stale `O(K)`

Pamatove naroky
- Kompaktne polia 
	- Zvycajne velke
	- Treba najst velke miesto, co sa nemusi podarit (hlavne v pripade fragmentovanej pamate)
- Nekompaktne 
	- Viacero alokacii mensich blokov (vo vseobecnosti rychlejsie ako alokovanie velkeho bloku)
	- Vyrazna nevyhoda - navyse alokovana pamat pre smerniky na dalsie polia
- Kompromis
	- Kombinacia tychto dvoch
	- Napr. trojrozmerne spravime ako jednorozmerne kompaktnych + dvojrozmerne nekompaktnych (alebo ina kombinacia)

#### Neregularne polia

"Zubate"  
Implementacne ako pole poli  

Problem s univerzalnou definiciou "zubatych" rozmerov, kedze vzdy moze byt ina  
- Pre 2D neregularne pole je to pole
- Pre 3D neregularne pole je to matica
- Pre KD neregularne pole je to (K-1)D pole vo vseobecnosti  

### Prioritne fronty

Kazdy prvok je charakterizovany svojou prioritou 

Priorita moze byt  

- Implicitna (ked prvok vstupy do fronty)
	- Zasobnik - vyssia cim neskor vstupi do struktury - papier do tlaciarne - LIFO
	- Front - naopak - rad v menze na obede - FIFO
- Explicitna - typicky vyjadrena nezapornym cislom, nizsie cislo = vyssia priorita, musi mat relacne operatory  
	- Prirodzena - prvok ma nejaku vlastnost, ktora moze byt pouzita ako priorita
	- Umela
	- Napr.
		- Sekvencny prioritny front
		- Dvojzoznam
		- Lavostranna halda

Priorita nikdy nie je zaporna, najvyssia priorita je 0  

#### Zasobnik

Dno zasobnika - tam kde padne prvy prvok ked ho vlozime  
To co vlozime to aj vyberame, **na tom istom konci**    
Je jedno, na ktorom konci sekvencie sa budu operacie vykonavat, ale musi to byt na tom istom konci  
Idealne implicitna alebo jednostranne zretazena sekvencia (bez referencie na posledny prvok)  
Explicitny vs. Implicitny zasobnik - Jednostranne zretazena sekvencia (operacie na zaciatku) vs. Implicitna sekvencia (bez referencie na posledny prvok, operacie na konci)

#### Front

Vkladame na jednom konci, na druhom konci vyberam  

Nie je mozne efektivne implementovat ako implicitnu sekvenciu s neobmedzenou kapacitou - linearne zlozitosti - nepripustne    
Idealne jednostranne zretazena sekvencia s referenciou na posledny prvok  
Dala by sa pouzit aj cyklicka implicintna sekvencia (pamatame si index kam mame vkladat, z kade mame vyberat a velkost, mame obmedzenu velkost)  

##### Implicitny front 
V obidvoch pripadoch problemovy
- Pri vkladani na zaciatok a vyberanie na konci - treba vsetko posuvat pri vkladani
- Pri vkladani na koniec a vybernie zo zaciatku - treba vsetko posuvat pri vyberani

Dalo by sa spravit, ze nebudeme posuvat pri vyberani zo zaciatku, a pamatat si poziciu zaciatku, ale toto by velmi plytvalo pamatou, zaciatok by sa stale len vzdaloval  
Dalo by sa vyriesit obmedzenim kapacity  
Ak dojde k naplneniu kapacity, prvky sa budu vkladat od zaciatku  
Cyklicka Implicitna Sekvencia  

#### Prioritny front

Prvky je mozne organizovat aj sekvencne aj hierarchicky  
Nie je definovana operacia porovnaj - neefektivne  

##### Sekvencny prioritny front  

Najjednoduchsia implementacia vklada prvky do sekvencie, a moze byt 
- Utriedena podla priorit - **Utriedeny sekvencny prioritny front** - pri vkladani sa vlozi na spravne miesto, vybera sa z konca - zlozitost `O(N)`
- Neutriedena podla priorit - **Neutriedeny sekvencny prioritny front** - pri vkladani sa vlozi na koniec (alebo zaciatok), pri vyberani sa musi hladat - zlozitost `O(N)`

Cize vzdy je jedna z operacii `vloz` alebo `vyber` so zlozitostou `O(N)` - nepripustne  

V praxi sa moc nepouziva, iba ako pomocne struktury v implementaciach prioritneho frontu  

#### Multistruktura Dvojzoznam

AUMS 

Pouziva dve sekvencie  
Vyuzijeme pozitivne vlastnosti sekvencnych prioritnych frontov - vkladanie do neutriedenych + vyberanie z utriedenych    
Kratka sekvencia, z ktorej sa lahko vybera - utriedena  
Dlha sekvencia, do ktorej sa lahko vklada (a nikdy nevybera) - neutriedena    

Kratka - obmedzena kapacita (vacsinou odmocnina z predpokladaneho max poctu prvkov), vkladame najmensie cisla = najvyssiu prioritu (zoradene), ak je plna tak najvacsie cislo vyhodime do dlhej  
Pri vyberani vyberieme z kratkej posledne cislo

Vyberanie - vzdy z kratkej, pretoze tu sa nachadzaju najvyssie priority, zlozitost `O(1)` ak nedojde k restrukturalizacii (popisane nizsie)

Vkladanie
- Do kratkej, ak ma prvok dostatocne vysoku prioritu
	- Ak kratka nie je plna
	- Iba ak tam na 100% patri  
	- Teda ak vyssiu ako najnizsi prvok v kratkej  
	- Zlozitost `O(m)`, teda dlzka kratkej sekvencie
- Ak je kratka sekvencia plna
	- Najnizsi prvok sa vlozi do dlhej
	- Vkladany prvok sa vlozi do kratkej
- Inak vkladam do dlhej

Ak je kratka prazdna - znova ju musim naplnit
- Prejst dlhu, najst najvyssie priority
- Pozor na zacyklenie - riesenie `novaDlha` sekvencia

**Restrukturalizacia**
- Velmi casovo narocna
- Dochadza k nej iba ak je kratka prazdna a treba ju naplnit
- Zlozitost `O(m * n)`
- Kedze k nej ale nedochadza tak casto, mozeme zlozitost **amortizovat** - "spriemerovat" na `O(m)`

#### Lavostranna Halda

*"Hald je cela halda"*

Uklada svoje prvky do implicitnej binarnej hierarchie  
Vklada/vybera sa vzdy iba na konci, tada uplne zlava (z tade nazov "lavostranna")  
Implicitna - neuchovava vztahy = pamatovo efektivna  

**Podmienka - Priorita otca je vyssia alebo rovna ako priorita oboch synov**  
Tym padom najvyssia priorita = koren  

Vkladanie
- Mozne iba na koniec (implicitna)  
 - Potrebujeme zabezpecit podmienku (priorita otca) 
	 - Cize ak nesedi momentalna pozicia tak sa vymenim s otcom 
 - Zlozitost = pocet urovni = $O(log_2(\text{pocet prvkov}))$  

![](aus-lavostranna-halda-1.png)
 
Mazanie 
- Da sa iba z posledneho miesta (listu) - implicitna
- Prehadzujeme so synom az kym sa nestane korenom 
- Ak vyberame koren - nahradime ho poslednym, poprehadzujeme synov (s najvyssou prioritou) 
- Zlozitost rovnaka = $O(log_2(\text{pocet prvkov}))$  

![](aus-lavostranna-halda-2.png)

Pristup k vrcholu O(1)  
> toto sa mi nejak nezda  

### Tabulky

Pristup na zaklade unikatneho kluca  
Kluc musi byt unikatny v ramci tabulky - dva prvky v tabulke nemozu mat rovnaky kluc  
Kluc moze byt  
- Prirodzeny
	- Prvok ma vlastnost, ktoru mozeme povazovat za kluc  
	- Musi mat relacne operatory
- Umely
	- Nema vlastnost ktoru mozeme povazovat za kluc, musime ho vytvorit

Operacie: vloz, najdi, **skus najst** (najuniverzalnejsia), obsahuje, vyber  

AUS sekvencna tabulka  
- Neutriedena podla klucov
	- Podobne ako neutriedeny sekvencny prioritny front
	- Ked najdeme prvok, mozeme prestat hladat (v NSPF sme museli vzdy prehladat cely)
	- V najhorsom pripade - nenajdeme aj po prehladani celej - zlozitost `O(n)`
	- Pri vymazavani prehodime vymazavany prvok s poslednym pre efektivnost
- Utriedena podla klucov
	- Prvok je potrebne vlozit na spravne miesto
	- Pri mazani nemozeme vymienat s poslednym 
	- Pomale operacie vloz a vyber kompenzuje velmi efektivny algoritmus na vyhladavanie
		- Polenie intervalov 
			- Pozrieme sa do polovice sekvencie
			- Ak sme nasli hladany prvok tak super
			- Ak sme nanasli hladany prvok tak pozreme ci je vacsi alebo mensi ako cislo v strede, podla toho rekurzivne vykonavame tento cyklus na zvysnej polovici az kym nenajdeme
			- Pristupit do polovice mozeme efektivne iba ak je implicitna
		- Zlozitost $O(log_2(n))$  

#### Triedenia sekvencnych tabuliek

Proces usporiadania prvkov tak, aby na boli v nami definovanom poradi  
Potrebuju efektivne pristupovat k prvkom struktury - O(1)  

Co sa vacsinou triedi - pole, implicitny zoznam, implicitna neutriedena sekvencna tabulka

Mozno usporiadavat sekvencne a hierarchicky

Operacia `skus_najst()`

**Rozdelenie**

- Podla umiestnenia struktury
	- Vnutorny - cela struktura v operacnej pamati
	- Vonkajsi - mimo operacnej pamate
- Podla efektivnosti triediaceho algoritmu
	- Prirodzeny - najrychlejsie utriedi uz utriedenu strukturu
	- Neprirodzeny - najpomalsie utriedi uz utriedenu strukturu
	- Neutralny - rychlost nezavisi od toho ci uz je alebo nie je utriedena
- Stabilita - triedenie nezmeni relativne poradie rovnakych prvkov
- Triedenie na mieste - nepotrebuje pomocnu sekvenciu alebo iny dalsi priestor

Triediace algoritmy su popisane nizsie v sekcii [Sorting](#Sorting)  

#### Tabulka s rozptylenymi zaznamami

Hashtable v Jave, Unordered map v C++

Hesovacia funkcia 
- Mlyncek 
- Pocita index, mapuje kluc do intervalu $<0; N>$, pricom $N$ = kapacita
- Musi byt rychla na vypocet
- Musi rovnomerne rozmiestnovat prvky v sekvencii (musi ich rozptylit, preto sa tabulka nazyva tak ako sa nazyva)  
- Musi minimalizovat kolizie 

Kolizia
- Nastava ked dva rozne kluce nam daju rovnaky hash = rovnake miesto v pamati  
- Taketo dva kluce potom nazyvame **synonyma**

Riesenie kolizii
- Zretazovanie
	- Na mieste vypocitanej adresy sa postupne vytvara sekvencia synonym
	- Mame 2 moznosti
		- Do bloku pamate pridat atribut reprezentujuci synonymum 
			- Z prvku `kluc`, `udaje` spravime prvok `kluc`, `udaje`, `synonymum`
			- Synonymum je pointer na dalsi prvok s rovnakym klucom po hashi 
		- Vyuzitie existujucej struktury
			- V tabulke su namiesto zaznamov pointre na ine sekvencie alebo dokonca tabulky
			- Pri vkladani/vyberani si pomocou hashu najdeme sekvenciu/tabulku ktoru poprosime o vlozenie/vybratie
			- Kusok viac pamatovo narocne
	- Ak budeme chciet pristupit k takemuto prkvu, tak prehladame dane synonyma/sekvenciu/tabulku a porovname realne hladany kluc
	- Stupa casova aj pamatova zlozitost
	- Ak sa toto bude diat velmi casto tak tabulka **zdegeneruje** na SLL alebo zretazenu tabulku
- Preplnovacia oblast
	- Podobne ako zretazovanie
	- Mame primarnu a preplnovaciu oblast, su bezprostredne za sebou
	- Pri kolizii vlozime novy prvok na prve volne miesto v preplnovacej oblasti
	- Prvkom okrem `klud` a `udaje` pridame aj `synonymum`, co bude
		- `-1` ak neexistuje synonymum
		- `cislo` - index v preplnovacej oblasti ak existuje synonymum
	- V podstate to iste, ale mame suvislu pamat
	- Je potrebne spravne zvolit velkost preplnovacej oblasti (malo moznych synonym vs. plytvanie pamatou)
	- Je potrebne predchadzat vzniku "dier" pri mazani synonym
- Opatovne hashovanie
	- Mame `M` hashovacich funkcii
	- Mame `M` oblasti - disjunktne mnoziny - implicitne sekvencie
	- Ak vznikne kolizia pri jednej hash funkcii, skusime druhu, skusime tretiu, ...
	- Ak vznikne kolizia aj pri poslednej hash funkcii tak koniec sveta
	- Problem ze ako zvolit velkost oblasti, plytvanie pamatou vs. nemame kam dat prvky
	- Rastie casova aj pamatova narocnost
- Otvorena adresacia
	- Ak trafime synonymum, najdeme prve volne miesto kam to vlozit
	- Toto sa nazyva sondovanie (probing)
	- Moze byt sekvencne (posuvam o `n` prvkov), linearne (posuvam o `n*m` prvkov),  kvadraticke (posuvam o `n^m` prvkov), ...
	- Clustre - zhlukovanie prvkov - napr. hash funkcia `kluc mod 20`
	- Problem - potrebovali by sme si ulozit info ze kolko prvkov chcelo byt vlozene na dane miesto

#### Binarny vyhladavaci strom

Tabulka, ktora je oragizovana ako usporiadany binarny strom (usporiadany podla klucov)  
Plati pravidlo **kluc laveho syna < kluc vrcholu < kluc praveho syna**  
Tym padom vsetky prvky v lavom podstrome su mensie, v pravom vacsie

**Pri vyhladavani** - **bisekcia** - skocim do stredu, zahodim polovicu v ktorej sa hodota urcite nenachadza, skocim do stredu toho co mi ostalo atd...  
Kuzelne su usporiadane pomocou inorder, lavy syn ma mensi kluc, pravy syn ma vacsi kluc  
Zlozitost zavisi od vyvazenia hierarchie - najlepsom pripade $O(log_2(n))$, v najhorsom pripade $O(n)$  

![](aus-bvs-1.png)

**Pri vkladani** je potrebne zachovat usporiadanie - pravidlo o velkosti klucov synov  
Namiesto posuvania vsetkeho (pri implicintej sekvencii) tu iba vlozime dalsieho syna poslednemu vrcholu - listu  
Problem ak vkladame vela klucov ktore su uz utriedene - dostaneme zdegenerovanu hierarchiu ktora ma len lavych/pravych synov - v podstate linked list  

**Pri mazani** je potrebne zachovat usporiadanie - pravidlo o velkosti klucov synov  
Najskor je potrebne vyhladat vrchol s danym klucom  
Nasledne mozu nastat 3 situacie pre takyto vrchol
- **Stupen je 0** - list - jednoducho ho dame prec
- **Stupen je 1** - prepojim svojho syna a svojho otca, poradie sa nepokazi
- **Stupen je 2**
	- Mazany vrchol bude nahradeny predchodcom alebo nasledovnikom v ramci inorder
	- Zistime ho tak ze pojdeme
		- Bud "raz dolava a potom furt doprava az ku listu"
		- Alebo naopak - "raz doprava a potom furt dolava az ku listu"
	- Tieto podstromy musia existovat, kedze ma mazany vrchol stupen aspon 2
	- Mazanie takymto stylom zachova poradie

| Stupen vymazavaneho vrcholu | Znazornenie mazania                      |
| --------------------------- | ---------------------------------------- |
| 0                           | ![](aus-bvs-2.png) |
| 1                           | ![](aus-bvs-3.png) |
| 2                           | ![](aus-bvs-4.png) |

##### Vyvazovanie
Aby sme mali co najvyvazenejsiu hierarchiu - aby sa nestalo/zredukovalo zdegenerovanie  
Da sa robit roznymi sposobmi

- Pridame do tabulky extra informaciu - Treap
- Pridame informaciu a topologiu struktury? - Cerveno-cierny strom, AVL strom
- Automaciky vyvazovanie - Splay strom

##### Rotacie
Ak nie je vyvazene, tak potrebujeme nieco odstat o uroven vyssie/nizsie
- Jednoducha lava rotacia vrcholu okolo svojho otca
- Jednoducha prava rotacia vrcholu okolo svojho otca
- Dalsie rotacie...

Ja (ako vrchol) sa chcem dostat o uroven vyssie   
Jedini dvaja dotknuti su moj otec a moj "brat"   
Iba prepointrovavam  
Z otca spravim syna   
Povodny brat bude syn povodneho otca   

![](aus-bvs-5.png)

#### Treap

Kombinacia Tree a Heap  
Kazdy prvok je okrem kluca a udajov navyse charakterizovany nahodnou prioritou  
Platia podmienky lavostrannej haldy - **Priorita otca je vyssia alebo rovna ako priorita oboch synov**  
Inak funguje tak isto ako BVS (ozaj?)
Kvalita vyvazenia sa bude odvijat od kvality generaovania nahodnych priorit  

## Sorting

### Sortovacie algoritmy

- Priame metody - zlozitost $O(n^2)$  
	- Select sort
	- Insert sort
	- Bubble sort
- Nepriame - zlozitost lepsia ako $O(n^2)$
	- Quick sort
	    - Bubble sort na steroidoch
	    - Rozdeli sa na 2 polovice
	    - Zvoli sa tzv. pivot - pomocna lokalna premenna, typicky stredny prvok
	    - Vsetko mensie ako pivot by malo byt nalavo, vsetko vacsie napravo
	    - Ak je dvojica prvkov na zlej strane tak ich swapneme
	    - Ked sa stretnem alebo prekrizim tak rozdelim tabulku na 2 a rekurzivne aplikujem Quick sort
	    - Ked dobre triafame pivotov tak zlozitost $O(n \times log_2(n))$, v najhorsom pripade $O(n^2)$  
		    - Zavisi od pivota - ak je pivot najmensi/najvacsi prvok tak je zle, ak je pivot median tak je super
	- Heap sort
	    - Pomocou akoze hierarchie pre predstavenie
	    - **Asi** sa robi take ze chceme na koren hierarchie dame najvacsie cislo a potom ho hodime na koniec, dalej pokracujeme len s neutriedenou castou
	    - Zlozitost $O(2 \times (n \times log_2(n)))$
	    - Na mieste, nestabilny, neutralny
	  - Shell sort
	  - Radix sort
	  - Merge sort


#### Select sort 

Na mieste, stabilny, neutralny

Tabulka je rozdelena na utriedenu a neutriedenu cast  
Z neutriedenej casti hladam najmensi prvok, a vlozim ho na koniec utriedenej casti  

> Actually sa nevyberaju a nevkladaju prvky, ale 2 sa navzajom vymenia

#### Insert sort 

Na mieste, stabily, prirodzeny

Tabulka je rozdelena na utriedenu a neutriedenu cast  
Z neutriedenej casti vyberam prvy prvok, a vkladam ho na na spravne miesto v utriedenej casti  

> Actually sa nevyberaju a nevkladaju prvky, ale 2 sa navzajom vymenia


#### Bubble sort 
Na mieste, staiblny, prirodzeny  

Vzdy porovnavam 2 prvky vedla seba  
- Ak su v spravnom poradi, tak sa posuniem dalej
- Ak nie su v spravnom poradi, tak ich vymenim a posuniem sa dalej

Toto sa opakuje az dovtedy kym tabulka nie je usporiadana  
Ak prejdeme celu tabulku, a nespravime ziadnu vymenu, tak uz je usporiadana

#### Shell sort

Vylepsuje insert sort  
Ciastocne sa triedi po 'krokoch'  
Zoradujeme prvky ktore su voci sebe vzdialene o velkost kroku  
Napr. krok 5 = zotriedime kazdych 5 krokov  
Krok sa znizuje, az kym nepride po 1

Podla toho aku sekvenciu krokov si zvolime - od $O(n^2)$ po $O(n^{\frac{4}{3}})$

#### Radix sort

To co triedime musi byt rozlozitelne - `int` na cifry, `string` na `char`, `osoba` sa neda - Komponent kluca  
Jednotlive casti musia nadobudat konecny pocet hodnot  
_este nieco dalsie_  
Zabera vela pamate  
Zlozitost _v podstate_ $O(n)$

Spravime _priehradku_ pre kazdy komponent kluca  
Nad kazdou priehradkou robime FIFO

...v podstate zoradime na urovni jednotiek, potom desiatok, potom stoviek

#### Merge sort

Zoberie "jednotice", usporiada do dvojic  
Zoberie dvojice, usporiada do stvoric  
Zoberie stvorice, usporiada do osmic...

Mam 2\*FIFO (stvorice) a vyberam mensi prvok, vkladam do osmice

Zlozitost $O(n * log_2(n))$

## Triedenie sekvencnych suborov

Mame velky subor a nezmesti sa do operacnej pamate  
Musime triedit citanim a zapisovanim na disk  
Treba minimalizovat tieto operacie

Triedenie prebieha pomocou **monotonii** a **buffera**  

Vytvorime buffer (pomocna sekvencia v operacnej pamati), a naplnime ho udajmi z disku (povodneho sekvencneho suboru)  
Vytvorime prvu prazdnu monotoniu - pomocny sekvencny subor ktoreho prvy neklesaju (rastu alebo su rovnake)  
Na zaciatku je monotonia prazdna, vkladame do nej vhodny udaj z buffera tak, aby boli v monotonii prvky usporiadane    
Porovnavanie nastava v bufferi - v operacnej pamati namiesto disku - rychlejsie  
Pri kazdom vybrati z buffera do neho vlozime novy prvok z povodneho sekvencneho suboru   

Ak sa uz v bufferi nenachadza vhodny prvok (a stale mame co triedit), tak uzavrieme prvu monotoniu  
Vytvori sa 2. monotonia a cyklus sa opakuje  

Ked uz vyprazdnime cely buffer a nemame ho cim naplnit tak koncime cyklus  
Udaje mame v ulozene monotoniach  
Nastava mergovanie monotonii  
Podobne ako pri merge sort - zo zaciatku dostupnych monotonii vyberieme najmensi prvok a zapiseme do sekvencneho suboru  
Ked vyprazdnime vsetky monotonie tak mame hotovo a utriedeny sekvencny subor   
 
## Paralelne prostriedky

Graficka karta  
Viacej vypoctov prebieha naraz (paralelne)  
Vypoctov musi byt vela a musia byt rovnake, a idealne co najrychlejsie na vypocet

**Komparacna siet** - skupina komparatorov a liniek  
**Komparator** - miesto, ktore rozhodne o poradi vstupov a zoradi ich v spravnom poradi na vystup... V podstate berie 2 hodnoty, a ak su v nespravnom poradi tak ich vymeni  
**Linky** - spajaju komparatory, vstupy a vystupy. Sluzia na prenos dat  

![](aus-komparacna-siet.png)

**Triediaca siet** - specialny typ komparacnej siete, ktora akykolvek vstup usporiada na vystupe  
*Nie kazda komparacna siet je aj triediaca siet*  
*Komparatory nesmu tvorit cyklicky graf*  

**Rodina** - skupina triediacich sieti, ktore pracuju na rovnakom principe, ale maju rozny pocet vstupov - napr. *Merger* moze mat 2 vstupy, 3 vstupy, ... Vo vseobecnosti oznacujeme ako *Merger\[n]*, pricom *n* je pocet liniek    

### Vykon triediacej siete

Komparatory pracuju v case $O(1)$  

**Hlbka triediacej siete** - v podstate kolko je najviac komparatorov za sebou, alebo za kolko sa vykona posledny komparator  
Vstupna linka ma hlbku $0$  
Vystupna linka komparatora ma hlbku $max(\text{predchadzajucaLinka}_1, \text{predchadzajucaLinka}_2) + 1$  
Cize v podstate za kazdym vystupom komparatora sa zvysi hlbka linky o 1  

![](aus-triediaca-siet.drawio.png)  

> Tie druhe komparatory dole su ohnute len preto aby bolo jasne vidiet ze ktore su spolu, ale stale pracuju sucasne  

Vykon triediacej siete bude teda $O(h)$  

Komparatory pracuju paralelne, cize vsetko co nie je presne za sebou pracuje naraz  
Predchadzajuci obrazok by teda mohol vyzerat aj nejak takto

Pri normalnom triedeni (neparalelnom, v operacnej pamati) sa snazime dosiahnut rychlosti $< O(n^2)$, idealne az $O(n)$  
Pri paralelnom triedeni sa snazime dosiahnut rychlosti $< O(n)$  

### Techniky tvorby triediacej siete  

Je matematicky dokazane, ze ak chceme zistit ci je siet triediaca, tak nemusime testovat vseky kombinacie, ale len vsetky kombinacie nul a jednotiek - **Princip 0-1**  
Odteraz uz budeme predpokladat len tuto metodu  

#### Insert sort (neefektivne)

![](aus-paralelny-insert.png)

#### Select sort (neefektivne)

V podstate je to to iste, len dole hlavou   
Spravi to to iste, tak isto rychlo (pomaly)  
Je to rovnako nepripustne neefektivne $O(n)$   

![](aus-paralelny-select.png)

#### Bitonic sort (efektivne)

**Bitonicka sekvencia** - sekvencia, ktorej prvky prave raz stupaju, a prave raz klesaju  
Bi-tonic - 2 tony - raz hore, raz dole  
Napr. *(1, 4, 6, 8 - 3, 2)*  
Takuto sekvenciu je mozne cyklicky posunut tak, aby sedela - z *(6, 9 - 4, 2 - 3, 5)* spravime *(2, 3, 5, 6, 9 - 4, 2)*  
Kedze mame *Princip 0-1*, tak by to mohlo vyzerat nejak takto *(0, 0, 1, 1, 1, 0, 0, 0)*  
Akakolvek dvojica je bitonicka sekvencia  
Akakolvek monotonna postupnost je bitonicka sekvencia  

**Bitonicky cista sekvencia** - ma rovnake prvky - iba nuly alebo iba jednotky - *(0, 0, 0, 0)*  
Kedze ma same rovnake prvky, tak mozeme povedat ze je utriedena  

Bitonic sort sa sklada z viac elementov - komparacnych sieti, ktore su popisane nizsie

##### Half-cleaner

Komparacna siet s hlbkou 1  
**Vstup** - bitonicka sekvencia  
**Vystup** - bitonicka sekvencia + bitonicky cista sekvencia  
Rychlost (hlbka) $O(1)$  

Half-cleaner - vycisti polovicu - z bitonickej sekvencie spravi polovicu *cistej* bitonickej sekvencie  

![](aus-bitonic1.png)  

![](aus-bitonic2.png)  

Ak chceme utriedit *celu* bitonicku sekvenciu, tak mozeme pridat dalsie Half-cleaners  
Tomuto sa hovori *Bitonic sorter*  

##### Bitonic sorter  

Nie bitonic sort  
Rekurzivny element (triediaca siet) skladajuci sa z Half-cleanerov  
**Vstup** - bitonicka sekvencia  
**Vystup** - utriedena sekvencia  
Rychlost (hlbka) $O(log_2 n)$  

V podstate len za sebou iduce Half-cleaners, vzdy o polovicu mensie  

![](aus-bitonic3.png)  

Teraz mame dalsi problem - ako dostat bitonicku sekvenciu  

##### Modifikovany Half-cleaner  

**Vstup** - dve utriedene sekvencie  
**Vystup** - dve bitonicke sekvencie  

Na vstupe mame jednu rastucu a druhu rastucu, na vystupe rasie-klesa, rastie-klesa   

![](aus-bitonic4.png)  

Mozeme zacat od uplne malych utriedenych sekvencii (velkost 1)  
Teraz ich potrebujeme spojit do jednej vacsej utriedenej sekvencie  
 
##### Merger 

Spoji 2 utriedene sekvencie do jednej  
**Vstup** - dve utriedene sekvencie  
**Vystup** - jedna velka utriedena sekvencia  

Vystupne bitonicke sekvencie Modifikovaneho Half-cleanera mozeme utriedit Bitonic sorterom  

![](aus-bitonic5.png)

##### Bitonic sort 

Nie Bitonic sorter  
**Vstup** - hocico  
**Vystup** - utriedena sekvencia  

Vyuziva mergery  
Ak zacneme uplne od najmensich utriedenych sekvencii (velkost 1) a pouzijeme *Merger\[2]*, tak dostaneme utriedenu sekvenciu velkosti 2  
Dve velkosti 2 mozeme zobrat a spojit do velkosti 4  
Dve velkosti 4 mozeme zobrat a spojit do velkosti 8  
Atd atd...

![](aus-bitonic6.png)  

Zlozitost (hlbka) - kazdy Merger je $O(log_2 n)$   
Kazdy jeden merger spusti 2 dalsie = $O(log_2 n) \times O(log_2 n) = O(log_{2}^{2} n)$   
Toto je rychlejsie ako $O(n)$, co v paralelnom svete potrebujeme  

V konecnom dosledku je to dookola stale vacsie a vacsie Modified HC - 2 Bitonic sortery

## Siete

### APT Siet

Bloky pamate sa oznacuju vrcholy  
Kazdy vrchol moze mat vztah s hocikym inym - vztahy `M:N`    
Ziadny vrchol nema vynimocne postavenie - vsetky su rovnocenne   

**Brana** - sekvencia vsetkych vrcholov, ktore sa nachadzaju v sieti  
Pomocou brany sa da dostat ku kazdemu vrcholu (prehliadky)  

**Stupen** vrcholu - pocet vztahov s inymi vrcholmi - kolko sipociek z neho ide  
**Velkost** siete - pocet vrcholov v sieti  

**Staticka siet** - brana je IS - neefektivne modifikatory, rychly pristup  
**Dynamicka siet** - brana je ES - efektivne modifikatory, pomaly pristup  

Zoznam vrcholov moze byt ulozeny v implicitnej alebo explicitnej sekvencii  

Samotne vrcholy (siet sama o sebe) nie je mozne implementovat efektivne v implicitnej sekvencii (nevieme pocet vztahov)  
Pri hierarchiach museli byt K-cestne, tu nic take nemame  

Celkovo mozeme mat teda 4 kombinacie  

![](aus-siete-1.png)

Vo vztahu nemusime mat len referenciu na vrchol, ale aj dodatocne udaje o vztahu - na "ciare medzi vrcholmi"   
Napr. cesta medzi dvoma mestami - vzdialenost, trieda, rychlost, ...   
Mame teda 2 udajove casti - jedna sa viaze k vrcholom, druha sa viaze ku vztahom medzi vrcholmi  

**Operacia *Prirad**** - obsah jednej siete (cielovej) nahradim obsahom druhej siete (zdrojovej)  
Treba zabezpecit deep copy  
Robi sa dvojkrokovo a nie velmi jednoducho - najprv shallow copy brany, potom prejdeme celu branu a "poopravujeme" referencie na spravne vrcholy (aby sa spravila deep copy zo shallow)  

**Dotaz *Existuje vztah**** - medzi dvoma vrcholmi - pozriet sa do vrcholu s mensim stupnom a pozriet ci ma vztah s tym druhym  

**Dotaz *Porovnaj***  
*"Ak ste si mysleli ze priradenie je neprijemne, tak porovnanie je neprijemne na druhu"*  
Porovnavame len udajove casti (?)  
Nie je mozne vykonat efektivne, je treba porovnavat prvky "kazdy s kazdym" a porovnavat ich vztahy, pricom nic nemusi byt v rovnakom poradi    
Je to velmi neefektivne, velmi velka zlozitost   
Treba robit "preconditions" - porovnat ci je rovnaky typ brany, uchovavajuci rovnake udaje, rovnake typy brany, rovnake typy vztahov, rovnake velkosti, rovnaky stupen, ...

Iteratory - cez branu, resp. iteratory sekvencie ktore reprezentuju branu    

### AUT Graf

Orientovany graf - jeho hrany su orientovane - nie je jedno ako smeruje sipka, resp. je vyslovene povedane z kade kam ide  
Ak by neboli sipocky ale iba ciarocky, alebo by boli obojstranne sipocky, tak graf je neorientovany  

Implementacia APT Siet  
Selektory - `spristupniVrcholZBrany`, `spristupniVrcholZVrcholu`, `spristupniNasledovnikov`, `spristupniPredchodcov`, `spristupniHranu`  
Dotazy - `stupen`, `existujeHrana`, `pocetHran`  
Modifikatory - `vlozVrchol`, `zrusVrchol`, `vlozHranu`, `zrusHranu`

### Implementacie AUT Graf

Vsetky dalsie grafy ako priklady budu vychadzat z tohto grafu   

![](aus-siete-3.png)

#### AUS Tabulka Hran 

Ukladame si len hrany v tabulke   
**Operacie na vrcholoch nie su definovane**  

V tabulke sa hrany mozu ukladat roznymi sposobmi
- Lexikograficky **ne**utriedena sekvencna implicitna tabulka hran
- Lexikograficky utriedena sekvencna implicitna tabulka hran
- Lexikograficky neutriedena sekvencna **explicitna** tabulka hran

![](aus-siete-4.png)

Kluc je hrana - napr. hrana `AB` smeruje z vrcholu `A` do `B` - cize len *"zoberieme nazov jedneho vrcholu a prilepime k nemu nazov druheho vrhcolu"*   
Ak su kluce lexikograficky utriedene, tak su bacially zoradene abecedne  

Pri lexikograficky utriedenej implementacii navyse mozeme vyhladavat bisekciou    

**Neexistuju operacie `spristupniVrcholZBrany` ani `spristupniVrcholZVrcholu`**, pretoze nemame info o vrcholoch, resp. nemame vrcholy  
Tiez nemame operacie `vlozVrchol` a `zrusVrchol`  

Vhodne iba pre male grafy  

#### Dvojurovnovy pristup (hviezda)

Dopredna hviezda - poznam vsetky vrcholy do ktorych smerujem - o tejto sa budeme bavit dalej    
> Spatna hviezda - poznam vsetky vrcholy ktore smeruju do mna  

Efektivny pristup   

**Prvotna struktura** - evidencia vrcholov  
**Druhotna struktura** - evidencia nasledovnikov vo vrchole  

Aj prvotnu aj druhotnu strukturu mozeme uchovavat v lubovolnej AUS  

##### Dvojurovnovy pristup - AUS Vseobecny Graf

**Vrcholovo staticka implementacia** = nie su definovane operacie `vlozVrchol` a `zrusVrchol`    
**Hranovo staticka implementacia** = nie su definovane operacie `vlozHranu` a `zrusHranu`  
Opakom su vrcholovo/hranovo dynamicke implementacie  

V priklade sa preberala matica - 2D pole  

**Jedine pole**  
V prvotnej strukture mame vsetky uzitocne prvky matice - tam kde je $-1$ alebo $\infty$ vynechame  
V druhotnej strukture si pamatame dlzky jednotlivych riadkov (kedze nie vsetky su rovnako dlhe, kedze vynechavame prazdne hodnoty)  

![](aus-siete-5.png)

Vieme sa dostat iba dopredu, nie dozadu   

#### Krizove reprezentacie

Tiez nazyvane ako *Presite pole*  
Dopredny aj spatny pristup  
Dve prvotne struktury a jednu (ciastocne zdielanu) druhotnu strukturu  

![](aus-siete-6.png)

Trieda `Hrana` - vie, ze odkade ide, aj kam (aj dopredu, aj naspat) (od koho ide, ku komu ide) *(?)*  

Na prvy pohlad... 
- Horizontalne po riadkoch (zelene) - prva prvotna struktura - `od 0`, `od 1`, `od 2`, `od 3`  
- Vertikalne po stlpcoch (cervene) - druha prvotna struktur - `do 0`, `do 1`, `do 2`, `do 3`  

Prva prvotna struktura uchovava doprednu hviezdu  
Druha prvotna struktura uchovava spatnu hviezdu  

> Neviem ci je spravne pomenovanie *prva* a *druha* prvotna struktura

Zelena - prvotna struktura doprednej hviezdy  
Ceverna - prvotna struktura spatnej hviezdy  

![](aus-siete-7.png)