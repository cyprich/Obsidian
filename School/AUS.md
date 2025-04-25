# AUS

Radsej pouzivat referenciu (`&`) ako pointer (`*`) ak sa da

- Jednoduchsia syntax (netreba dereferencovat)
- Zabera menej pamate
- Je to v podstate nieco ako alias?

```c++
int x = 20;
int* ptr = &x;  // pointer
int& ref = x;  // referencia
```

Pri ASCII stringu: pocet pismenok = pocet bajtov  
Pri Unicode stringu: pocet pismenok sa nemusi byt pocet bajtov (UTF-16 pismenko = dvojbajt, UTF-32 pimenko = stvorbajt)

Najlepsie pouzivat UTF-8

Primitivne typy (int, short, long,...) vs. Odvodene typy (class, struct)

```c++
enum WeekDay: short {
    Monday,
    Tuesday,
    // ...
    Sunday
};
```

---

## Pamat

### Zasobnik - Call stack

Malicky  
Stack pointer - vrchol zachobnika - tam kde zasobnik konci  
Deli sa na ramce (frames)

- Novy ramec sa vytvori vzdy ked sa zavola nejaka funkcia?
- Znici sa ked sa skonci funkcia (`}`)

My default na windowse `1MB`, stack overflow - "minul" sa zasobnik

Odovzdavanie parametrov funkcii

- Hodnotou - pass by value
  - `void print(int val) {...}`
  - Implicitne alokuje novu pamat pre dany typ (int `4B`, class `vela`), pozor na velke typy (class, pole)
- Adresou - pass by address
  - `void print(int* val) {...}`
  - Implicitne alokuje novu pamat pre pointer (`8B`)
  - Trebe dereferencovat `{ (*val) += 5; }`
- Odkazom - pass by refrence
  - `void print(int& val) {...}`
  - Netreba dereferencovat `{ val += 5; }`
  - Implicitne alokuje novy pamat pre pointer (`8B`)
  - Da sa zabranit modifikaciam `void print (const int& val) {...}`
  - Kedze referencia je synonimum, tak

Prudko nepouzivat `mutable` a `const cast`

### Halda - Heap

Velka kapacita, (skoro) vsetka ostatna pamat

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

Smart pointers - unique pointers,

### Spravca pamate

- Zodpovedny za pridelovanie a vratenie pamate
- Udrziava informaciu o
- First-fit (rychlejsie) vs. Best-fit (menej fragmentacie) vs. Worst-fit (mam zoznam volneho miesta, pouzijem prve = najvacsie -> najrychlejsie)
- Objektove typy
  - Alokuje pamat + vola konstruktor
  - Pri destrukcii najprv zavola destruktor a potom uvolni pamat

#### Expanzna strategia

- konstantna - $m + 10$
- linearna - $m * 2$
- geometricka - $m ^2$

##

### Logicke usporiadanie udajovych struktur

![obrazok](../others/images/aus_logicke_usporiadanie_stuktur.png)

- Sekvencia (asi pole) - 1 predchodca, 1 nasledovnik - `1:1`
- Hierarchia (vyzera ako strom) - 1 predchodca, n nasledovnickov `1:n`
- Siet (total mess) - m predchodcov, n nasledovnikov - `m:n`

### Abstraktny pamatovy typ (APT)

Rozhranie, ktore urci ako su organizovane bloky v pamati (sekvencne, hierarchicky, siet)

### Abstraktna pamatova struktura (APS)

Kombinuje APT a spravcu pamate

- Sekvencia
  - Implicitna sekvencia + Spravca suvislej pamate
  - Obojstranne zretazena sekvencia + Spravca suvislej pamate
  - Obojstranne zretazena sekvencia + Spravca nesuvislej pamate
  - `...`
- Hierarchia
  - `...`

Problem - strasne vela kombinacii  
Riesenie - kompozicia (no idea what that is, informatika 1 flashbacks) namiesto dedicnosti

Mozu byt implicitne (v pamati za sebou - compact memory manager) a explicitne (rozhadzane v pamati)

### Iteratory

_"Iterator je prst"_  
Ukazuje na nejaku poziciu v strukture a moze sa pohybovat na dalsie miesto  
Zapuzdruje \_  
Musi byt fess rychly - zlozitost `O(1)` (skoro vzdy)

Typy

- Standardny
- Dopredny - viem citat od niekam po niekam
- Obojsmerny - viem posuvat aj dozadu
- S nahodnym pristupom - posun sa o niekolko

Prehliadky

## Sekvencia

Linearny pamatovy abstraktny typ  
Jeden predchodca, jeden nasledovnik

## Hierarchia

Stromova struktura  
Kazdy blok pamate sa oznacuje ako vrchol

### Terminologia

Otec, syn  
Predok, potomok  
Koren, list

Stupen = pocet synov  
Uroven = ako daleko od korena som (koren = 0, jeho synovia = 1, ...)  
Hlbka = najvacsia uroven  
Mohutnost = pocet vrcholov

### Klasifikacia

Viaccestne hierarchie

- Pocet synov vrcholu nie je obmedzeny
- Napr. suborovy system

K-cestne

- Binarne - K=2 - vrchol moze mat max 2 synov
- Trojcestne
- Stvorcestne
- Dalej sa delia na usporiadane a neusporiadane
  - Usporiadane - synov je mozne pomenovat nejakou vlastnostou - najstarsi, najmladsi, lavy, pravy, ...
  - Neusporiadane

### Vlastnosti

Vyvazena hierarchia - vsetky listy v hierarchii lezia takmer v rovnakej hlbke

Pre K-cestne

- Kompletnost - zaplname hierarchiu postupne - nevytvarame novu uroven, ak este momentalna uroven nie je uplne zaplnena
- Plnost - kazby vrchol ma prave `0` alebo `n` synov
- Perfektnost - je kompletne zaplnena, vsetky listy su na jednej urovni, taky pekny trojuholnik

### Prehliadky

Je mozne prehliadat do hlbky a do sirky

- Prehliadka do hlbky
  - Prehliadka v priamom poradi (preorder) - najprv spracujem seba, potom synov - basically prehladavame od korena smerom dole k listom
  - Prehliadka v spatnom poradi (postorder) - najprv spracujem synov, potom seba - bsically prehladavame od listov smerom hore ku korenu
  - Specialny pripad - Prehladka vo vnutornom poradi (inorder)
    - Iba v Binarnej hierarchii
    - Ak mam laveho syna tak ho spracujem, potom spracujem seba, potom praveho syna
    - V konecnom dosleku spracujem celu mapu zlava doprava
  - Max. pocet ramcov (rekurzia) = hlbka
- Prehliadka do sirky (level order) - po jednotlivych urovniach - nie je rekurzivna (max. 1 ramec)

### Dotazy

JeKoren (SpristupniOtca, ak je null)  
JeSyn ()

## Udajove struktury

Abstraktnu udajovy typ - AUT - podobne ako pamatovy, ale uz s konkretnymi datami/hodnotami?  
Datova cast APT (bez vztahovej casti)

Nie vsetko podporuje iterovanie - nie ze by sa nedalo, ale nedavalo by zmysel  
Iteratory sa mozu zdedit okrem metody `spristupni()`, z ktorej musime vratit len data

Maju typ `<T>` namiesto `<TypBloku>`

### Konkretne AUT

- Zoznam
  - Basically len obalka sekvencie
  - Moze byt napr ArrayList (ArrayList v jednom jazyku nemusi byt to iste ako ArrayList v druhom jazyku)
  - Pristup na zaklade indexu
  - Specialny typ - cyklicky zoznam - prvy a posledny prvok su prepojene
  - Podporuje iterovanie
  - `VseobecnyZoznam<T, TypSekvencie>`
  - ImplicitnyZoznam, JednostranneZretazenyZoznam, JednostranneZretazenyZoznam, + Cyklicka verzia ku kazdemu
- Strom
  - Obalka hierarchie
  - `VseobecnyStrom<T, TypHierarchie>`

## Polia

Fixna velkost po dobu celeho zivotneho cyklu, mozeme len vyberat/prepisovat prvky (nie vkladat/odoberat)  
Pristup pomocou indexu, moze byt 2-zlozkovy, 3-zlozkovy, K-zlozkovy
Pri vzniku musime naplnit - definovat hodnotu prvkom (kostruktor pri objektoch)  
Regularne (plne) vs. neregularne (chybaju prvky, vnorene pole ma menej elementov)

### Regularne

Mapovanie po riadkoch, po stlpcoch

Lexikograficke poradie  
Kolexikograficke poradie

#### Kompaktne

Zlozitost O(1) pri jednorozmernych  
Pri viacrozmernych O(K^2) kde K je pocet rozmerov  
Ked upravime vzorec, tak dostaneme zlozitost O(K), ale ten nechceme implementovat  
"Vzdy je to rozdiel\*nieco + rozdiel\*nieco + rozdiel\*nieco + rozdiel\*1"  
**Informacny vektor** - predpocitame si nieco co sa nemeni  
Informacny vektor je ze (N3xN2, N3, 1) = (15, 3, 1)  
Potom spravime dot product informacny vektor a (index - baza)

**!!! TOTO SA BUDU PYTAT NA SKUSKE !!!**

#### Nekompaktne

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

### Neregularne

Zubate polia  
To iste ako regularne nekompaktne - s jednym issue - dlzka jednotlivych poli, ako to popisat  
K-rozmerne neregularne pole musime popisat (K-1)-rozmernym polom

## Prioritne fronty

Priorita

- Implicinta (ked prvok vstupy do fronty)
  - Zasobnik - vyssia cim neskor vstupi do struktury - papier do tlaciarne - LIFO
  - Front - naopak - rad v menze na obede - FIFO
- Explicitna - typicky vyjadrena cislom
  - Sekvencny prioritny fromt
    - Prirodzena - prvok ma prirodzene nejaky atribut, podla ktoreho sa to jasne urcuje
    - Umela
  - Dvojzoznam
  - Lavostranna halda

### Zasobnik

Dno zasobnika - tam kde padne prvy prvok ked ho vlozime  
To co vlozime to aj vyberame, na tom istom konci  
Idealne implicitna alebo jednostranne zretazena (bez referencie na posledny prvok) sekvencia

### Front

Vkladame na jednom konci, na druhom konci vyberam  
Idealne jednostranne zretazena sekvencia s referenciou na posledny prvok  
Dala by sa pouzit aj cyklicka implicintna sekvencia (pamatame si index kam mame vkladat a z kade mame vyberat a velkost, mame obmedzenu velkost)  
Explicitny vs. Implicitny front

### Prioritny front

Priorita nikdy nie je zaporna, najvyssia priorita je 0  
Nie je definovana operacia porovnaj  
Sekvencne - utriedene aj neutriedene ma nepripustne zlozitosti (na jednej strane pomale vkladanie, na druhej pomale vyberanie) - v praxi sa moc nepouziva?

### Multistruktura Dvojzoznam

Vkladanie do neutriedenych + vyberanie z utriedenych  
Kratka sekvencia, z ktorej sa lahko vybera  
Dlha sekvencia, do kotrej sa lahko vklada (a nikdy nevybera)

Kratka - obmedzena kapacita (vacsinou odmocnina z predpokladaneho max poctu prvkov), vkladame najmensie cisla (zoradene), ak je plna tak najvacsie cislo vyhodime do dlhej  
Pri vyberani vyberieme z kratkej posledne cislo

Do kratkej vkladam iba ak:

- kym kratka nie je zaplnena, predtym ako bola zaplnena
- ak uz bola zaplnena, tak vlozim iba taky prvok ktory tam na 100% patri

Ak je kratka prazdna, tak musime prejst dlhu a znova naplnit prazdnu (pozor na zacyklenie - riesenie `nova dlha`) - **restrukturalizacia**, zlozitost O(m \* n) - mozeme **amortizovat** - spravit priemer - dostaneme **O(m)**

### Lavostranna Halda

_Hald je cela halda_

Uklada svoje prvky do hierarchie  
Implicitna hierarchia  
Binarny strom

**Podmienka - Priorita otca je vyssia alebo rovna ako priorita oboch synov**  
Tym padom najvyssia priorita = koren  
Vkladat mozeme iba na koniec (implicitna)  
Vkladanie - Potrebujeme zabezpecit podmienku (priorita otca), cize ak nesedi tak sa vymenim s otcom - zlozitost = pocet urovni = O(log2(pocet prvkov))  
Mazanie - da sa iba z posledneho miesta (listu), vyberame koren - nahradime ho poslednym, poprehadzujeme synov (s najvyssou prioritou) - zlozitost rovnaka  
Pristup k vrcholu O(1)

## Tabulky

Pristup na zaklade unikatneho kluca  
Operacie: vloz, najdi, skus najst (najuniverzalnejsia), obsahuje, vyber

AUS sekvencna tabulka  
AUS utriedena sekvencna tabulka - velmi rychla, ale nie pri vkladani nie

### Triedenia sekvencnych tabuliek

Proces usporiadania prvkov tak, aby na konci boli v nami definovanom poradi  
Potrebuju efektivne pristupovat k prvkom struktury - O(1)  
Co sa vacsinou triedi - pole, implicitny zoznam, implicitna neutreidena sekvencna tabulka

Rozdelenie

- Podla umiestnenia struktury
  - Vnutorny - cela struktura v operacnej pamati
  - Vonkajsi - mimo operacnej pamate
- Podla efektivnosti triediaceho algoritmu
  - Prirodzeny - najrychlejsie utriedi uz utriedenu strukturu
  - Neprirodzeny - najpomalsie...
  - Neutralny - ryzhlost nezavisi od otho ci uz je alebo nie je utriedena
- Stabilita - triedenie podla viac parametrov nezmeni poradie prveho parametra?
- Triedenie na mieste - nepotrebuje pomocnu sekvenciu?

#### Algoritmy

- Priame metody - zlozitost O(n^2)
  - Select sort
    - Hladam najmensi prvok a vymenim ho z prvym
    - Na mieste, stabilny, neutralny
  - Insert sort
    - Swapujem smerom dozadu az kym nie je na spravnom mieste
    - Na mieste, stabily, prirodzeny
  - Bubble
    - Vymienam po dvoch az do konca, stale dookola (vzdy od zaciatku) az kym ine je utriedeny
    - Ak nebola ziadna vymena tak je utrieene
    - Staiblne, na mieste, prirodzeny
- Nepriame - zlozitost lepsia ako O(n^2)
  - Quick
    - Bubble sort na steroidoch
    - Rozdeli sa na 2 polovice
    - Zvoli sa tzv. pivot - pomocna lokalna premenna, typicky stredny prvok
    - Vsetko mensie ako pivot by malo byt nalavo, vsetko vacsie napravo
    - Ak je dvojica prvkov na zlej strane tak ich swapneme
    - Ked sa stretnem alebo prekrizim tak rozdelim tabulku na 2 a rekurzivne aplikujem Quick sort
    - ????
    - Ked dobre triafame pivotov tak zlozitost O(n \* log2(n)), v najhorsom pripade O(n^2)
      - Zavisi od pivota - ak je pivot najmensi/najvacsi prvok tak je zle, ak je pivot median tak je super
  - Heap
    - Pomocou akoze hierarchie pre predstavenie
    - **Asi** za robi take ze chceme na koren hierarchie dame najvacsie cislo a potom ho hodime na koniec, dalej pokracujeme len s neutriedenou castou
    - O(2(n \* log(n)))
    - Na mieste, nestabilne, neutralny
  - Shell
  - Radix
  - Merge
