# AUS - pseudokody, operacie, selektory, modifikacie, ...

Doplnenie k suboru [AUS](./AUS.md)  
Tu budu pseudokody a take tie vsetky mozne tabulecky, grafy a operacie co som nechcel pchat do povodneho suboru AUS  
Pseudokody budu take polopseudokody, poloC++

## Casove zlozitosti 

| $f(N)$            | Horny asymptoticky odhad | Nazov triedy zlozitosti | Priklady                                                                                                                                                         |
| ----------------- | ------------------------ | ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $1$               | $O(1)$                   | Konstantna              | Pristup k prvku v IS, vlozenie prvku do ES, pristup ku korenu hierarchie, zistenie poctu vrcholov siete, vlozenie prvku do hashtable                             |
| $N$               | $O(N)$                   | Linearna                | Pristup k prvku v ES, vymazanie celej siete, vlozenie prvku do utriedenej sekvencnej tabulky, vypocet mohutnosti hierarchie, priehradkove triedenie (Radix sort) |
| $N^2$             | $O(N^2)$                 | Kvadraticka             | Porovnanie dvoch sieti, Insert sort, Select sort, Bubble sort                                                                                                    |
| $N^k$             | $O(N^k)$                 | Polynomialna            |                                                                                                                                                                  |
| $log(N)$          | $O(log(N))$              | Logaritmicka            | Vyhladanie prvku v BVS, vyhladanie prvku v Treap, vlozenie prvku do lavostrannej haldy, vyhladanie prvku v utriedenej sekvencnej tabulke                         |
| $n \times log(n)$ | $O(n \times log(n))$     | *"Linearitmicka"*       | Triedenie haldou (Heap sort), triedenie spajanim (Merge sort)                                                                                                    |
| $a^n$             | $O(a^n)$                 | Exponencialna           |                                                                                                                                                                  |

## Spravca pamate

Zodpovedny za pridelovanie a vratenie pamate  
Udrziava zaznamy o polohe a velkosti volnych segementov pamate  

Operacie spravcu pamate

| Nazov operacie          | Parametere   | Navratova hodnota | Vyznam                                                             |
| ----------------------- | ------------ | ----------------- | ------------------------------------------------------------------ |
| Pridel Pamat            | `int`        | `*TypBloku`       | Vyhradi pamat pre konkretny pocet blokov, vrati referenciu na prvy |
| Uvolni pamat            | `*TypBloku`  |                   | Uvolni bloky pamate umiestnene na odovzdanej adrese                |
| Uvolni a vynuluj        | `**TypBloku` |                   | Uvolni odovzdane bloky, parameter nastavi na `null`                |
| Daj pocet alokovanych   |              | `int`             | Vrati pocet alokovanych blokov pamate                              |
| Daj velkost alokovanych |              | `int`             | Vrati velkost alokovanej pamate                                    |

Operacie kompaktnej pamate

| Nazov operacie      | Parametre                                                                     | Navratova hodnota | Vyznam                                                                                |
| ------------------- | ----------------------------------------------------------------------------- | ----------------- | ------------------------------------------------------------------------------------- |
| Velkost pamate      | `referencia` alebo `udajovy typ`                                              | `int`             | Vrati velkost (pocet bajtov) pamate odovzdanej ako parameter                          |
| Zmen velkost pamate | `referencia`, `int`                                                           | `referencia`      | Zvacsi alebo zmensi velkost odovzdanej pamate na novu hodnotu                         |
| Skopiruj pamat      | `referencia`, `referencia`, `int`                                             |                   | Prekopiruje pocet bajtov z jedneho miesta na druhe, **nekontroluje** prekrytie pamate |
| Premiestni pamat    | `referencia`, `referencia`, `int`                                             |                   | Prekopiruje pocet bajtov z jedneho miesta na druhe, **kontroluje** prekrytie pamate   |
| Porovnaj            | `referencia`, `referencia`, `int`                                             | `bool`            | `true` ak sa dany pocet bajtov od pociatocnych referencii zhoduje                     |
| Prepis              | `referencia`, `char`, `int`                                                   |                   | Pozadovany pocet krat skopiruje hodnotu jedneho bajtu na od danej referencie *(?)*    |
| Vymen               | (`referencia`, `referencia`, `int`)  <br>alebo (`udajovy typ`, `udajovy typ`) |                   | Vymeni obsah dvoch pamatovych miest                                                   |

Dalsie operacie spravcu kompaktnej pamate

| Nazov operacie    | Parametre   | Navratova hodnota | Vyznam                                            |
| ----------------- | ------------ | ----------------- | ------------------------------------------------- |
| Vypocitaj adresu  | `int`        | `*TypBloku`       | Vypocita adresu bloku pamate s danym poradim      |
| Vypocitaj poradie | `*TypBloku`  | `int`             | Vypocita poradie daneho bloku v kompaktnej pamati |
| Daj blok pamate   | `int`        | `*TypBloku`       | Vrati blok pamate s danym poradim                 |
| Vymen             | `int`, `int` |                   | Navzajom vymeni bloky pamate s danym poradim      |

## APT - Abstraktny Pamatovy Typ

Zakladne operacie

| Nazov operacie | Parametre | Navratova hodnota | Vyznam                    |
| -------------- | ---------- | ----------------- | ------------------------- |
| Prirad         | `*APT`     | `*APT`            | Priradi prvky z ineho APT |
| Vymaz          |            |                   | Vymaze vsetky prvky z APT |

Dotazy 

| Nazov operacie | Parametre | Navratova hodnota | Vyznam                                          |
| -------------- | ---------- | ----------------- | ----------------------------------------------- |
| Porovnaj       | `*APT`     | `bool`            | `true`, ak maju APT rovnaky obsah, inak `false` |
| Velkost        |            | `int`             | Aktualny pocet prvkov v APT                     |
| Je prazdny     |            | `bool`            | `true`, ak sa v APT nenachadzaju ziadne prvky   |

### APT Sekvencia

Zakladne operacie

| Nazov operacie  | Parametre  | Navratova hodnota | Vyznam                                             |
| --------------- | ----------- | ----------------- | -------------------------------------------------- |
| Vypocitaj index | `*TypBloku` | `int`             | Vrati index bloku pamate odovzdaneho ako parameter |

Selektory

| Nazov selektora           | Parametre  | Navratova hodnota | Vyznam                                   |
| ------------------------- | ----------- | ----------------- | ---------------------------------------- |
| Spristupni prvy           |             | `*TypBloku`       | Prvy blok pamate sekvencie               |
| Spristupni posledny       |             | `*TypBloku`       | Posledny blok pamate sekvencie           |
| Spristupni                | `int`       | `*TypBloku`       | Blok pamate s danym indexom              |
| Spristupni nasledujuci    | `*TypBloku` | `*TypBloku`       | Nasledujuci blok ku bloku v parametri    |
| Spristupni predchadzajuci | `*TypBloku` | `*TypBloku`       | Predchadzajuci blok ku bloku v parametri |

Modifikatory

| Nazov modifikatora | Parametre  | Navratova hodnota | Vyznam                          |
| ------------------ | ----------- | ----------------- | ------------------------------- |
| Vloz prvy          |             | `*TypBloku`       | Vlozi na zaciatok sekvencie     |
| Vloz posledny      |             | `*TypBloku`       | Vlozi na koniec sekvencie       |
| Vloz               | `int`       | `*TypBloku`       | Vlozi na miesto podle parametra |
| Vloz za            | `*TypBloku` | `*TypBloku`       | Vlozi za blok v parametri       |
| Vloz pred          | `*TypBloku` | `*TypBloku`       | Vlozi pred blok v paramteri     |

Prehliadky

| Nazov prehliadky                      | Parametre                       | Navratova hodnota | Vyznam                                                                                  |
| ------------------------------------- | -------------------------------- | ----------------- | --------------------------------------------------------------------------------------- |
| Spracuj v priamom poradi              | `*TypBloku`, `lambda(*TypBloku)` |                   | Vyvola funkciu `lambda` nad blokmi v priamom poradi, pricom zacina na bloku z parametra |
| Spracuj v spatnom poradi              | `*TypBloku`, `lambda(*TypBloku)` |                   | Vyvola funkciu `lambda` nad blokmi v spatnom poradi, pricom zacina na bloku z parametra |
| Spracuj vsetky bloky v priamom poradi | `lambda(*TypBloku)`              |                   | Vyvola funkciu `lambda` nad kazdym blokom sekvencie v priamom poradi                    |
| Spracuj vsetky bloky v spatnom poradi | `lambda(*TypBloku)`              |                   | Vyvola funkciu `lambda` nad kazdym blokom sekvencie v spatnom poradi                    |

### APT Hierarchia

Selektory

| Nazov selektora  | Parametre          | Navratova hodnota | Vyznam                                            |
| ---------------- | ------------------ | ----------------- | ------------------------------------------------- |
| Spristupni koren |                    | `*TypBloku`       | Spristupni koren hierarchie                       |
| Spristupni otca  | `*TypBloku`        | `*TypBloku`       | Spristupni otca bloku v parametri                 |
| Spristupni syna  | `*TypBloku`, `int` | `*TypBloku`       | Spristupni syna bloku v parametri s danym poradim |

Dotazy

| Nazov dotazu   | Parametre          | Navratova hodnota | Zlozitost | Parametre zlozitosti                | Vyznam                                |
| -------------- | ------------------ | ----------------- | --------- | ----------------------------------- | ------------------------------------- |
| Uroven         | `*TypBloku`        | `int`             | $O(h)$    | **$h$** = hlbka hierarchie          | Vrati uroven hierarchie               |
| Stupen         | `*TypBloku`        | `int`             |           |                                     | Vrati stupen hierarchie               |
| Mohutnost      | `*TypBloku`        | `int`             | $O(n)$    | **$n$** = pocet prvkov v hierarchii | Vrati mohutnost hierarchie            |
| Velkost        |                    | `int`             | $O(n)$    | **$n$** = pocet prvkov v hierarchii | Vrati velkost hierarchie              |
| Je koren       | `*TypBloku`        | `bool`            | $O(1)$    |                                     | `true`, ak je dany vrchol koren       |
| Je N-ty syn    | `*TypBloku`, `int` | `bool`            | $O(1)$    |                                     | `true`, ak je vrchol n-tym synom otca |
| Je list        | `*TypBloku`        | `bool`            |           |                                     | `true`, ak je vrchol listom           |
| Ma N-teho syna | `*TypBloku`, `int` | `bool`            | $O(1)$    |                                     | `true`, ak ma vrchol n-teho syna      |
| Je prazdny     |                    | `bool`            | $O(1)$    |                                     | `true`, ak je prazdna                 |

Modifikatory

| Nazov modifikatora | Parametre                       | Navratova hodnota | Vyznam                                          |
| ------------------ | ------------------------------- | ----------------- | ----------------------------------------------- |
| Vloz koren         |                                 | `*TypBloku`       | Vlozi novy koren, povodny dealokuje             |
| Zmen koren         | `*TypBloku`                     |                   | Zmeni koren na novy, povodny dealokuje          |
| Vloz syna          | `*TypBloku`, `int`              | `*TypBloku`       | Vlozi noveho syna na miesto s danym poradim     |
| Zmen syna          | `*TypBloku`, `int`, `*TypBloku` |                   | Zmeni syna s danym poradim, podovneho dealokuje |
| Zrus syna          | `*TypBloku`, `int`              |                   | Zrusi syna s danym poradim                      |

Prehliadky

| Nazov prehliadky                        | Parametre                        | Princip                                                                                            |
| --------------------------------------- | -------------------------------- | -------------------------------------------------------------------------------------------------- |
| Spracuj v priamom poradi                | `*TypBloku`, `lambda(*TypBloku)` | Spracuje vrcholy v priamom poradi od bloku v parametri az po koniec operaciou `lambda` v parametri |
| Spracuj v spatnom poradi                | `*TypBloku`, `lambda(*TypBloku)` | Spracuje vrcholy v spatnom poradi od bloku v parametri az po koniec operaciou `lambda` v parametri |
| Spracuj po urovniach                    | `*TypBloku`, `lambda(*TypBloku)` | Spracuje vrcholy po urovniach od bloku v parametri az po koniec operaciou `lambda` v parametri     |
| Spracuj vsetky vrcholy v priamom poradi | `lambda(*TypBloku)`              | Spracuje vsetky vrcholy operaciou `lambda` v priamom poradi                                        |
| Spracuj vsetky vrcholy v spatnom poradi | `lambda(*TypBloku)`              | Spracuje vsetky vrcholy operaciou `lambda` v spatnom poradi                                        |
| Spracuj vsetky vrcholy po urovniach     | `lambda(*TypBloku)`              | Spracuje vsetky vrcholy operaciou `lambda` po urovniach                                            |

#### Binarna hierarchia  

Binarna hierarchia je specialnym pripadom K-cestnej hierarchie, kde `K=2`, pricom `K` je ako parameter  

Selektory

| Nazov selektora         | Parametre   | Navratova hodnota | Vyznam                                                  |
| ----------------------- | ----------- | ----------------- | ------------------------------------------------------- |
| Spristupni laveho syna  | `*TypBloku` | `*TypBloku`       | Spristupni laveho syna bloku odovzdaneho ako parameter  |
| Spristupni praveho syna | `*TypBloku` | `*TypBloku`       | Spristupni praveho syna bloku odovzdaneho ako parameter |

Dotazy

| Nazov dotazu    | Parametre   | Navratova hodnota | Vyznam                                        |
| --------------- | ----------- | ----------------- | --------------------------------------------- |
| Je lavy syn     | `*TypBloku` | `bool`            | `true`, ak je vrchol lavym synom svojho otca  |
| Je rpavy syn    | `*TypBloku` | `bool`            | `true`, ak je vrchol pravym synom svojho otca |
| Ma laveho syna  | `*TypBloku` | `bool`            | `true`, ak ma vrchol laveho syna              |
| Ma praveho syna | `*TypBloku` | `bool`            | `true`, ak ma vrchol praveho syna             |

Modifikatory  
> Tak isto to vyzera pre pravych synov

| Nazov modifikatora | Parametre                | Navratova hodnota | Vyznam                                 |
| ------------------ | ------------------------ | ----------------- | -------------------------------------- |
| Vloz laveho syna   | `*TypBloku`              | `*TypBloku`       | Vlozi otcovi laveho syna               |
| Zmen laveho syna   | `*TypBloku`, `*TypBloku` |                   | Zmani laveho syna, povodneho dealokuje |
| Zrus laveho syna   | `*TypBloku`              |                   | Zrusi laveho syna vrcholu              |

#### Iteratory

Univerzalne iteratory do hlbky  

| Nazov operacie           | Parametre   | Navratova hodnota | Vyznam                                                                                                                                                              |
| ------------------------ | ----------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Uloz poziciu             | `*TypBloku` |                   | Vytvori a uchova objekt `PoziciaIteratorDoHlbky` zodpovedajuci spracovaniu vrcholu odovzdavaneho ako parameter                                                      |
| Odstran poziciu          |             |                   | Zrusi prvy objekt `PoziciaIteratorDoHlbky` v postupnosti                                                                                                            |
| Skus najst dalsieho syna |             | `bool`            | Posunie index aktualne spracovavaneho syna vrcholu v objekte `PoziciaIteratoraDoHlbky` na dalsieho platneho syna. Vrati `true`, ak bolo mozne najst dalsieho syna   |

| Nazov operacie | Parametre | Navratova hodnota | Vyznam |
| -------------- | --------- | ----------------- | ------ |
|                |           |                   |        |

| Nazov operacie | Parametre | Navratova hodnota | Vyznam |
| -------------- | --------- | ----------------- | ------ |
|                |           |                   |        |

| Nazov operacie | Parametre | Navratova hodnota | Vyznam |
| -------------- | --------- | ----------------- | ------ |
|                |           |                   |        |



#### Pseudokody - prehliadky Hierarchii

Prehliadky v priamom poradi *(preorder)*

```c++
void Hierarchia<TypBloku>.spracujVPriamomPoradi(*TypBloku vrchol, lambda(*TypBloku) operacia) {
	if (vrchol != nullptr) {
		operacia(vrchol);

		int stupenVrcholu = stupen(vrchol*);
		int poradieSyna = 0;
		int pocetSpracovanychSynov = 0;

		while (pocetSpracovanychSynov < stupenVrcholu) {
			TypBloku* syn = spristupniSyna(vrchol*, poradieSyna);
			if (syn != nullptr) {
				spracujVPriamomPoradi(syn, operacia);
				++pocetSpracovanychSynov;
			}
			++poradieSyna;
		}
	}
}

void Hierarchia<TypBloku>.spracujVsetkyVrcholyVPriamomPoradi(lambda(*TypBloku)) {
	spracujVPriamomPoradi(spristupniKoren, operacia);
}
```

Prehliadky v spatnom poradi *(postorder)*

```c++
void Hierarchia<TypBloku>.spracujVSpatnomPoradi(*TypBloku vrchol, lambda(*TypBloku) operacia) {
	if (vrchol != nullptr) {
		int stupenVrcholu = stupen(vrchol*);
		int poradieSyna = 0;
		int pocetSpracovanychSynov = 0;

		while (pocetSpracovanychSynov < stupenVrcholu) {
			TypBloku* syn = spristupniSyna(vrchol*, poradieSyna);
			if (syn != nullptr) {
				spracujVSpatnomPoradi(syn, operacia);
				++pocetSpracovanychSynov;
			}
			++poradieSyna;
		}
		operacia(vrchol);
	}
}

void Hierarchia<TypBloku>.spracujVsetkyBlokuVSpatnomPoradi(lambda(*TypBloku) operacia) {
	spracujVSpatnomPoradi(spristupniKoren(), operacia);
}
```

Prehliadka po urovniach *(levelorder)*

```c++
void Hierarchia<TypBloku>.spracujPoUrovniach(*TypBloku vrchol, lambda(*TypBloku) operacia) {
	if (vrchol != nullptr) {
		JZS<*TypBloku> sekvencia = JZS<*TypBloku>;
		sekvencia.vlozPrvy()->data = vrchol;
		while (!sekvencia.jePrazdny()) {
			*TypBloku aktualny = sekvencia.spristupniPrvy()->data;
			sekvencia.zrusPrvy();
			if (aktualny != nullptr) { 
				operacia(aktualny);
				int stupenVrcholu = stupen(aktualny*);
				for (int n = 0; n < stupenVrcholu - 1; ++n) {
					sekvencia.vlozPosledny()->data = spristupniSyna(aktualny*, n);
				}
			}
		}
	}
}

void Hierarchia<TypBloku>.spracujVsetkyVrcholyPoUrovniach(lambda(*TypBloku) operacia) {
	spracujPoUrovniach(spristupniKoren(), operacia);
}
```

Pri binarnej hierarchii je jeden dalsi specialny typ prehliadky - vo vnutornom poradi *(inorder)*  

```c++
void BinarnaHierarchia<TypBloku>.spracujVoVnutornomPoradi(*TypBloku vrchol, lambda(*TypBloku) operacia) {
	if (vrchol != nullptr) {
		spracujVoVnutornomPoradi(spristupniLavehoSyna(vrchol*, operacia));
		operacia(vrchol);
		spracujVoVnutornomPoradi(spristupniLavehoSyna(vrchol*, operacia));
	}
}

void BinarnaHierarchia<TypBloku>.spracujVsetkyVrcholuVoVnutornomPoradi(lambda(*TypBloku) operacia) {
	spracujVoVnutornomPoradi(spristupniKoren(), operacia)
}
```

### APT Siet

Selektory

| Nazov selektora             | Parametre          | Navratova hodnota | Vyznam                                                         |
| --------------------------- | ------------------ | ----------------- | -------------------------------------------------------------- |
| Spristupni vrchol z brany   | `int`              | `*TypBloku`       | Spristupni vrchol z brany podla jeho poradia                   |
| Spristupni vrchol z vrcholu | `*TypBloku`, `int` | `*TypBloku`       | Spristupni vrchol z ineho vrcholu podla jeho poradia vo vztahu |

Dotazy

| Nazov dotazu   | Parametre                | Navratova hodnota | Vyznam                                             |
| -------------- | ------------------------ | ----------------- | -------------------------------------------------- |
| Stupen         | `*TypBloku`              | `int`             | Stupen vrcholu (pocet vztahov) vrcholu v parametri |
| Existuje vztah | `*TypBloku`, `*TypBloku` | `bool`            | `true`, ak vrcholy maju medzi sebou vztah          |
| Pocet vztahov  |                          | `int`             | Pocet vztahov v celej sieti                        |

Modifikatory

| Nazov modifikatora | Parametre                | Navratova hodnota | Vyznam                                     |
| ------------------ | ------------------------ | ----------------- | ------------------------------------------ |
| Vloz               |                          | `*TypBloku`       | Vlozi novy vrchol do siete                 |
| Zrus               | `*TypBloku`              |                   | Zrusi/odstrani vrchol zo siete             |
| Spoj               | `*TypBloku`, `*TypBloku` |                   | Spoji - vytvori vztah medzi dvoma vrcholmi |
| Odpoj              | `*TypBloku`, `*TypBloku` |                   | Odpoji - zrusi vztah medzi dvoma vrchomi   |

Zlozitosti 

| Operacia/Selektor/Dotaz/Modifikator | Zlozitost                                                                                                       | Parametre                                                                                      |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| Prirad                              | $O(v) + O(w) * O(n)$                                                                                            | $v$ = pocet povodnych vztahov  <br>$w$ = pocet novych vztahov  <br>$n$ = pocet novych vrcholov |
| Vymaz                               | $O(v)$                                                                                                          | $v$ = pocet vztahov                                                                            |
| Spristupni vrchol z brany           | $O(1)$ ak je brana IS  <br>$O(n)$ ak je brana JZS  <br>$O \left( \dfrac{n}{2} \right)$ ak je brana OZS          | $n$ = pocet prvkov brany                                                                       |
| Spristupni vrchol z vrcholu         | $O(1)$ ak su vztahy v IS  <br>$O(s)$ ak su vztahy v JZS  <br>$O \left( \dfrac{s}{2} \right)$ ak su vztahy v OZS | $s$ = stupen vrcholu                                                                           |
| Porovnaj                            | $O(v^2)$                                                                                                        | $v$ = pocet vztahov                                                                            |
| Velkost                             | $O(1)$                                                                                                          |                                                                                                |
| Stupen                              | $O(1)$                                                                                                          |                                                                                                |
| Existuje vztah                      | $O(s)$                                                                                                          | $s$ = stupen vrcholu                                                                           |
| Pocet vztahov                       | $O(n)$                                                                                                          | $n$ = pocet prvkov                                                                             |
| Vloz                                | $O(1)$                                                                                                          |                                                                                                |
| Zrus                                | $O(n) \times O(s^2)$                                                                                            | $n$ = pocet prvkov  <br>$s$ = stupen vrcholu                                                   |
| Spoj                                | $O(1)$                                                                                                          |                                                                                                |
| Odpoj                               | $2 \times O(s)$                                                                                                 | $s$ = stupen vrcholu                                                                           |

## APS - Abstraktna Pamatova Struktura

Vsetky iteratory APS

| Nazov operacie | Parametre | Navratova hodnota | Vyznam                                                      |
| -------------- | ---------- | ----------------- | ----------------------------------------------------------- |
| Vpred          |            |                   | Efektivne posunie iterator na nasledujuci prvok v strukture |
| Spristupni     |            | `*TypBloku`       | Spristupni aktualny prvok v strukture                       |
| Ma dalsi       |            | `bool`            | `true`, ak existuje dalsi prvok                             |

Dopredne iteratory APS 

| Nazov operacie | Parametre                    | Navratova hodnota | Vyznam                                          |
| -------------- | ----------------------------- | ----------------- | ----------------------------------------------- |
| Je rovnaky     | `*DoprednyIterator<TypBloku>` | `bool`            | `true`, ak ukazuju na rovnaky prvok struktury   |
| Je rozny       | `*DoprednyIterator<TypBloku>` | `bool`            | `true`, ak ukazuju na rozdielny prvok struktury |

Obojsmerne iteratory APS 

| Nazov operacie    | Parametre | Navratova hodnota | Vyznam                                                         |
| ----------------- | ---------- | ----------------- | -------------------------------------------------------------- |
| Vzad              |            |                   | Efektivne posunie iterator na predchadzajuci prvok v strukture |
| Ma predchadzajuci |            | `bool`            | `true`, ak existuje predchadzajuci prvok v strukture           |

Iteratory s nahodnym pristupom APS 

| Nazov operacie | Parametre                              | Navratova hodnota | Vyznam                                                                        |
| -------------- | --------------------------------------- | ----------------- | ----------------------------------------------------------------------------- |
| Je pred        | `*IteratorSNahodnymPristupom<TypBloku>` | `bool`            | `true`, ak ukazuje na prvok pred iteratorom odovzdanym ako parameter          |
| Je za          | `*IteratorSNahodnymPristupom<TypBloku>` | `bool`            | `true`, ak ukazuje na prvok za iteratorom odovzdanym ako parameter            |
| Posun sa o     | `int`                                   |                   | Efektivne posunie iterator na prvok vzdialeny o parameter od aktualneho prvku |

Metody struktury pre `for-each` cyklus v C++

| Nazov operacie | Parametre | Navratova hodnota | Vyznam                                                      |
| -------------- | ---------- | ----------------- | ----------------------------------------------------------- |
| `begin`        |            | iterator          | Vrati iterator na zaciatok sekvencie                        |
| `end`          |            | iterator          | Vrati itertor na koniec sekvencie - prvy **neplatny** prvok |

Operatory iteratora pre `for-each` cyklus v C++

| Nazov operacie | Parametre | Navratova hodnota | Vyznam                                                 |
| -------------- | ---------- | ----------------- | ------------------------------------------------------ |
| `operator!=`   | iterator   | `bool`            | `true`, ak sa iteratory nezhoduju - operacia `jeRozny` |
| `operator*`    |            | `T`               | Spristupni aktualny prvok - operacia `spristupni`      |
| `operator++`   |            | iterator`&`       | Posunie sa dalej - operacia `vpred`                    |

Sekvencne prehliadky APS s vyuzitim iteratorov - zlozitosti


| Prehliadka                  | Zlozitost          | Parametre                                                                                                                  |
| --------------------------- | ------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| Spracuj vsetky bloky        | $O(n) \times O(a)$ | **n** - pocet blokov pamate spracovanych prehliadkou  <br>**$O(a)$** - zlozitost operacie spracovania jedneho bloku pamate |
| Najdi blok s vlastnostou    | $O(n) \times O(k)$ | **n** - pocet blokov pamate spracovanych prehliadkou  <br>**$O(k)$** - zlozitost kontroly jedneho bloku pamate             |
| Poradie bloku s vlastnostou | $O(n) \times O(k)$ | **n** - pocet blokov pamate spracovanych prehliadkou  <br>**$O(k)$** - zlozitost kontroly jedneho bloku pamate             |


### APS Implicitna sekvencia


| Nazov operacie     | Parametre | Navratova hodnota | Vyznam                   |
| ------------------ | ---------- | ----------------- | ------------------------ |
| Index nasledovnika | `int`      | `int`             | Vrati index nasledovnika |
| Index predchodcu   | `int`      | `int`             | Vrati index predchodcu   |

Selektory

| Nazov operacie            | Parametre | Zlozitost |
| ------------------------- | ---------- | --------- |
| Spristupni prvy           |            | $O(1)$    |
| Spristupni posledny       |            | $O(1)$    |
| Spristupni                |            | $O(1)$    |
| Spristupni nasledujuci    |            | $O(1)$    |
| Spristupni predchadzajuci |            | $O(1)$    |

Modifikatory

| Nazov operacie    | Parametre                                                                                                                   | Zlozitost       |     |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------- | --------------- | --- |
| Vloz prvy         | **b** = $\lceil \dfrac{n}{B} \rceil$  <br>**$B$** = velkost buffera (kolko prvkov sa don zmesti)  <br>**$n$** = pocet prvkov | $2 \times O(b)$ |     |
| Vloz posledny     | *to iste*                                                                                                                    | $O(b)$          |     |
| Vloz              | *to iste*                                                                                                                    | $2 \times O(b)$ |     |
| Vloz za           | *to iste*                                                                                                                    | $2 \times O(b)$ |     |
| Vloz pred         | *to iste*                                                                                                                    | $2 \times O(b)$ |     |
| Zrus prvy         | *to iste*                                                                                                                    | $O(b)$          |     |
| Zrus posledny     | *to iste*                                                                                                                    | $O(1)$          |     |
| Zrus              | *to iste*                                                                                                                    | $O(b)$          |     |
| Zrus nasledovnika | *to iste*                                                                                                                    | $O(b)$          |     |
| Zrus predchodcu   | *to iste*                                                                                                                    | $O(b)$          |     |

### APS Explicitna sekvencia

Pomocne operacie + ich zlozitosti

| Nazov operacie  | Parametre operacie       | Navratova hodnota | Zlozitost                                                                   | Parametre zlozitosti                                            | Vyznam                                                                 |
| --------------- | ------------------------ | ----------------- | --------------------------------------------------------------------------- | --------------------------------------------------------------- | ---------------------------------------------------------------------- |
| Spoj bloky      | `*TypBloku`, `*TypBloku` |                   | $O(1)$                                                                      |                                                                 | Navzajom prepoji 2 bloky pamate                                        |
| Odpoj blok      | `*TypBloku`              |                   | $O(1)$ ak je obojstranne zretazena  <br>$O(n)$ ak je jednostranne zretazena | **$n$** = pocet prvkov                                          | Vypoji blok z retazca blokov                                           |
| Prirad          | `*APT`                   | `*APT`            | $O(n + m)$                                                                  | **$n$** = povodny pocet prvkov  <br>**$m$** = novy pocet prvkov | Vymazu sa povodne udaje, a nahradia sa udajmi zo struktury v parametri |
| Vymaz           |                          |                   | $O(n)$                                                                      | **$n$** = pocet prvkov                                          | Vymaze vsetky prvky struktury                                          |
| Porovnaj        | `*APT`                   | `bool`            | $O(n)$                                                                      | **$n$** = pocet prvkov                                          | `true`, ak su dve struktury rovnake                                    |
| Vypocitaj index | `*TypBloku`              | `int`             | $O(n)$                                                                      | **$n$** = pocet prvkov                                          | Vrati index bloku z parametra                                          |

Selektory - zlozitosti  
> Parameter $n$ = pocet prvkov sekvencie

| Operacia                  | Zlozitost                                                                                 |
| ------------------------- | ----------------------------------------------------------------------------------------- |
| Spristupni prvy           | $O(1)$                                                                                    |
| Spristupni posledny       | $O(1)$ ak ma referenciu na posledny  <br>$O(n)$ ak nema referenciu na posledny            |
| Spristupni                | $\dfrac{O(n)}{2}$ ak ma referenciu na posledny  <br>$O(n)$ ak nema referenciu na posledny |
| Spristupni nasledujuci    | $O(1)$                                                                                    |
| Spristupni predchadzajuci | $O(1)$ ak je obojstranne zretazena  <br>$O(n)$ ak je jednostranne zretazena               |

Explicitna sekvencia v kompaktnej pamati - modifikator oprav spojenie  

| Nazov operacie | Parametre | Navratova hodnota | Vyznam                                                                           |
| -------------- | ---------- | ----------------- | -------------------------------------------------------------------------------- |
| Oprav Spojenie | `int`      |                   | Opravi vztahy svojho nasledovnika a predchodcu na zmenenu adresu podla parametra |

Modifikatory - zlozitosti  
> Parameter $n$ = pocet prvkov sekvencie

| Operacia      | Zlozitost                                                                                 |
| ------------- | ----------------------------------------------------------------------------------------- |
| Vloz prvy     | $O(1)$                                                                                    |
| Vloz posledny | $O(1)$ ak ma referenciu na posledny  <br>$O(n)$ ak nema referenciu na posledny            |
| Vloz          | $\dfrac{O(n)}{2}$ ak ma referenciu na posledny  <br>$O(n)$ ak nema referenciu na posledny |
| Vloz za       | $O(1)$                                                                                    |
| Vloz pred     | $O(1)$ ak je obojstranne zretazena  <br>$O(n)$ ak je jednostranne zretazena               |

### APS Implicitna hierarchia

Musi byt v kompaktnej pamati, musi byt usporiadana, musi byt K-cestna  

Operacie

| Nazov operacie | Parametre                                              | Navratova hodnota | Vyznam                                     |
| -------------- | ------------------------------------------------------- | ----------------- | ------------------------------------------ |
| Index otca     | `int` alebo `*ImplicitnaAPS.TypBloku`                   | `int`             | Vrati index otca vrcholu v suvislej pamati |
| Index syna     | (`int`, `int`) alebo (`*ImplicitnaAPS.TypBloku`, `int`) | `int`             | Vrati index syna v suvislej pamati         |

Selektory

| Nazov operacie           | Navratova hodnota                | Zlozitost | Vyznam                                  |
| ------------------------ | -------------------------------- | --------- | --------------------------------------- |
| Spristupni posledny list | `*ImplicitnaAPS.TypBloku`        | $O(1)$    | List na poslednej urovni najviac vpravo |
| Spristupni koren         |                                  | $O(1)$    |                                         |
| Spristupni otca          | `*ImplicitnaAPS.TypBloku`        | $O(1)$    |                                         |
| Spristupni syna          | `*ImplicitnaAPS.TypBloku`, `int` | $O(1)$    |                                         |

Dotazy

| Nazov operacie | Zlozitost                                                        | Parametre                                     |
| -------------- | ---------------------------------------------------------------- | --------------------------------------------- |
| Uroven         | $O(1)$                                                           |                                               |
| Stupen         | $O(1)$                                                           |                                               |
| Mohutnost      | $O(1)$ ak je vrchol korenom  <br>$O(n)$ ak vrchol nie je korenom | **$n$** = pocet vrcholov v hierarchii vrcholu |

Modifikatory  
> Vkladanie len na uplnom konci  

| Nazov operacie     | Zlozitost | Parametre                                                                                         |
| ------------------ | --------- | ------------------------------------------------------------------------------------------------- |
| Vloz posledny list | $O(b)$    | **$b$** $= \lceil \dfrac{n}{B} \rceil$  <br>**$B$** = velkost buffera  <br>**$n$** = pocet prvkov |
| Zrus posledny list | $O(1)$    |                                                                                                   |

### APS Explicitna hierarchia  

Operacie

| Nazov operacie   | Zlozitost                                                                                     | Parametre                                               |
| ---------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| Prirad           | $O(n + m)$                                                                                    | $n$ = povodny pocet prvkov  <br>$m$ = novy pocet prvkov |
| Vymaz            | $O(n)$                                                                                        | $n) = pocet prvkov                                      |

Selektory

| Nazov operacie   | Parametre                                                                                     | Navratova hodnota    |
| ---------------- | --------------------------------------------------------------------------------------------- | -------------------- |
| Spristupni otca  | $O(1)$                                                                                        |                      |
| Spristupni koren | $O(1)$                                                                                        |                      |
| Spristupni syna  | $O(1)$ ak su synovia ulozeni v IS, alebo su vymenovani  <br>$O(s)$ ak su synovia ulozeni v ES | $s$ = stupen vrcholu |

Dotazy

| Nazov operacie | Zlozitost                                                                                   | Parametre          |
| -------------- | ------------------------------------------------------------------------------------------- | ------------------ |
| Porovnaj       | $O(n)$                                                                                      | $n$ = pocet prvkov |
| Stupen         | $O(1)$ v neusporiadanych hierarchiach  <br>$O(K)$ v usporiadanych $K$-cestnych hierarchiach |                    |

Modifikatory 


| Operacia   | Zlozitost                                                                       | Parametre                                                                                    |
| ---------- | ------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Vloz koren | $O(1)$                                                                          |                                                                                              |
| Zmen koren | $O(1)$                                                                          |                                                                                              |
| Vloz syna  | $O(s)$                                                                          | $s$ = stupen vrcholu, synovia su ulozeni v sekvencii, pripustne su iba viaccestne hierarchie |
| Zmen syna  | $O(1)$ ak su synovia ulozeni v IS  <br>$O(s) ak su synovia ulozeni v ES         | $s$ = stupen vrcholu                                                                         |
| Zrus syna  | $O(m)$ ak su synovia ulozeni v IS  <br>$O(s) + O(m)$ ak su synovia ulozeni v ES | $s$ = stupen vrcholu  <br>$m$ = mohutnost podhierarchie mazaneho syna                        |

## AUT

### AUT Strom

Selektory

| Nazov selektora  | Parametre        | Navratova hodnota |
| ---------------- | ---------------- | ----------------- |
| Spristupni koren |                  | `*Vrchol`         |
| Spristupni otca  | `*Vrchol`        | `*Vrchol`         |
| Spristupni syna  | `*Vrchol`, `int` | `*Vrchol`         |

Dotazy

| Nazov dotazu   | Parametre        | Navratova hodnota |
| -------------- | ---------------- | ----------------- |
| Uroven         | `*Vrchol`        | `int`             |
| Stupen         | `*Vrchol`        | `int`             |
| Mohutnost      | `*Vrchol`        | `int`             |
| Je koren       | `*Vrchol`        | `bool`            |
| Je N-ty syn    | `*Vrchol`, `int` | `bool`            |
| Je list        | `*Vrchol`        | `bool`            |
| Ma N-teho syna | `*Vrchol`, `int` | `bool`            |

Modifikatory

| Nazov modifkatora | Parametre  | Navratova hodnota |
| ----------------- | ---------- | ----------------- |
| Vloz prvy         | `T`        |                   |
| Vloz posledny     | `T`        |                   |
| Vloz              | `int`, `T` |                   |
| Nastav            | `int`, `T` |                   |
| Zrus prvy         |            |                   |
| Zrus posledny     |            |                   |
| Zrus              | `int`      |                   |


## AUS

### AUS Vseobecny Zoznam

Zlozitosti vychadzaju zo zlozitosti pouzitej sekvencie (implicitna, explicitna jednostranne zretazena, explicitna obojstranne zretazena)  

### AUS Vseobecny Strom

Zlozitosti vychadzaju zo zlozitosti pouzitej hierarchie  

![](aus-vseobecny-strom-zlozitosti.png)


### AUS Viacrozmerne pole

#### AUS Viacrozmerne regularne pole

Selektory

| Nazov Selektora | Parametre                | Navratova hodnota | Vyznam                           |
| --------------- | ------------------------ | ----------------- | -------------------------------- |
| Spristupni      | `int`, `int`, ..., `int` | `T`               | Spristupni prvok s danym indexom |

Modifikatory

| Nazov operacie | Parametre                     | Navratova hodnota | Vyznam                                        |
| -------------- | ----------------------------- | ----------------- | --------------------------------------------- |
| Nastav         | `T`, `int`, `int`, ..., `int` |                   | Nastavi prvok s danym indexom na novu hodnotu |

Dotazy

| Nazov operacie   | Parametre | Navratova hodnota | Vyznam                       |
| ---------------- | --------- | ----------------- | ---------------------------- |
| Pocet dimenzii   |           | `int`             | Vrati pocet dimenzii         |
| Baza dimenzie    | `int`     | `int`             | Vrati bazu danej dimenzie    |
| Velkost dimenzie | `int`     | `int`             | Vrati velkost danej dimenzie |

### AUS Binarny vyhladavaci strom

#### Pseudokody - rotacie

```c++
void VseobecnyBVS.rotujDolava(TypVrcholuBVS* vrchol) {
	TypVrcholuBVS* lavySyn = vrchol->lavy;
	TypVrcholuBVS* otec = vrchol->otec;
	TypVrcholuBVS* praotec = otec->otec;

	pamatovaStruktura->zmenPravehoSyna(otec*, nullptr);
	pamatovaStruktura->zmenLavehoSyna(vrchol*, nullptr);

	if (praotec != nullptr) {
		if (praotec->lavy == otec) {
			pamatovaStruktura.zmenLavehoSyna(praotec*, vrchol);
		} else {
			pamatovaStruktura.zmenPravehoSyna(praotec*, vrchol);
		}
	} else {
		pamatovaStruktura->zmenKoren(vrchol);
	}

	pamatovaStruktura->zmenPravehoSyna(otec*, lavySyn);
	pamatovaStruktura->zmenLavehoSyna(vrchol*, otec);
}

void VseobecnyBVS.rotujDolava(TypVrcholuBVS* vrchol) {
	TypVrcholuBVS* pravySyn = vrchol->pravy;
	TypVrcholuBVS* otec = vrchol->otec;
	TypVrcholuBVS* praotec = otec->otec;

	pamatovaStruktura->zmenLavehoSyna(otec*, nullptr);
	pamatovaStruktura->zmenPravehoSyna(vrchol*, nullptr);

	if (praotec != nullptr) {
		if (praotec->lavy == otec) {
			pamatovaStruktura.zmenLavehoSyna(praotec*, vrchol);
		} else {
			pamatovaStruktura.zmenPravehoSyna(praotec*, vrchol);
		}
	} else {
		pamatovaStruktura->zmenKoren(vrchol);
	}

	pamatovaStruktura->zmenLavehoSyna(otec*, pravySyn);
	pamatovaStruktura->zmenPravehoSyna(vrchol*, otec);
}
```


| Nazov operacie | Parametre | Navratova hodnota | Vyznam |
| -------------- | ---------- | ----------------- | ------ |
|                |            |                   |        |

| Nazov operacie | Parametre | Navratova hodnota | Vyznam |
| -------------- | ---------- | ----------------- | ------ |
|                |            |                   |        |

| Nazov operacie | Parametre | Navratova hodnota | Vyznam |
| -------------- | ---------- | ----------------- | ------ |
|                |            |                   |        |

