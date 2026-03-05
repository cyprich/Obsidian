# PANS

Principy a Aplikacie Neuronovych Sieti

Hodnotenie

- Semester 50
  - 10 python kurz
  - 20 male online testy (dokopy 4)
  - 15 challenge solving
  - 5 projekt
- Skuska 50
  - 30 prakticka cast
  - 20 ustna cast

## Uvod

### AI vs. ML vs. DL

Slovník Merriam-Webster - artificial intelligence (noun): the capability of computer systems or algorithms to imitate intelligent human behavior

Artificial Inteligence

- Vseobecny pojem pre vsetko dalsie
- Stroj sa sprava podobne ako keby mal ludske myslenie
- Tu nemame ziadne data
- Priklad
  - Jednoduchy SPAM filter - dame si pravidlo ze chceme filtrovat spravy ktore obsahuju slovo Free

Machine Learning

- Trochu zlozitejsie
- **Natrenovane** na datach
- **Strukturovane data s priznakmi**
- My urcujeme priznaky
- Zlozitejsi spam filter - na zaklade viac parametrov + dame sadu sprav na ktorej si to natrenuje

Deep Learning

- Najzlozitejsie
- Nestrukturovane - text, audio, video
- Dame stroju vela vela dat, povieme mu ktore spravy su spam a ktore nie, on si sam pride na to ako to bude vyhodnocovat

DL je podkategoria ML  
ML je podkategoria AI

Este podkategoria DL - Generativna AI - vstup moze byt vsetko, vystup moze byt vsetko - chatgpt

### Ulohy pre AI

Klasifikacia - predpovedam triedu  
Regresia - predpovedam cislo  
Zhlukovanie (clustering) - nemam stitky/znacky/labels

### Typy ucenia

Supervised

- Mam k dispozicie dvojice `X` a `Y`
  - `X` su data
  - `Y` su stitky

Unsupervised

- Mam k dispozicii len `X`

`X` je to co viem zmerat
`Y` je to co by som chcel zistit

Unsupervised

- Klasifikacia
- Regresia
- Clustrovanie

### Pipeline praca ML

1. Ziskam data
2. Spracujem ich, vyrobim priznaky
3. Natrenujem model
4. Vyhodnotim
5. Opakujem dookola - mozem zmenit data, priznaky, model

### Rozdelenie dat

Trenovacia cast

- Trenujem na nej

Validacna cast

- Not sure co to je, iba si myslim
- Ked uz mam natrenovane tak ako keby overim ci som natrenoval dobre
- Nasledne mozem poupravovat veci

Testovacia cast

- Tieto data nevidim pri treningu
- "Svaty dataset"
- Az tu sa uplne zisti ze ci je nas model dobry alebo je natrenovany len na trenovacie a validacne data

Priklad na testoch

- Trenovacia cast - ucebne materialy, prednasky, skripta, ...
- Validacna cast
  - Testy od spoluziakov z minulych rokov
  - Mozem si otestovat ci som sa dobre naucil
  - Ale to ze budem vediet odpovede na tieto otazky mi este nezarucuje ze budem vediet odpvoedat aj v skole na teste (ucitel moze zmenit test)
- Testovacia cast
  - Actually test/skuska v skole
  - Az tu sa zisti ci som sa naozaj dobre naucil

Mozu byt rozdelene v pomere napr. 60%/20%/20%

### Baseline algoritmy

#### Linearna regresia

$y \approx w \times x + b$

Zvolime "dato" na zaklade linearnej regresie  
Hladam priamku/rovinu, ktora najlepsie sedi na datach

Interpretovatelny - vieme povedat co jednotlive prvky vyjadruju

Mozeme mat napr $\hat{y} = w_1 x_1 + w_2 x_2 + b$

- $\hat{y}$ = odhadovana cena bytu
- $w_1$ = cena za $1 m^2$
- $x_1$ = celkova rozloha v $m^2$
- $w_2$ = cena za jednu izbu
- $x_2$ = celkovy pocet izieb
- $b$ = konstanta (casto nic nevyjadruje, nema zmysel)

Hladame toto: $min \sum_{i} (y_i - \hat{y}_i)^2$ - cize taku priamku kde rozdiel medzi nameranymi datami a touto priamkou bude co najmensi

Z poskytnutych dat sme linearnou regresiou zistili ze $w_1 \approx 1.8$, $w_2 \approx 21.1$ (v tisicoch eur), $b = -4.5$

Priklad s bytom (urcujeme cenu bytu na zaklade rozlohy a poctu izieb), BMI, glukoza, ...

#### k-NN

`k` Nearest Neighbors - `k` Najblizsich Susedov  
Hyperparameter `k` = pocet susedov

Ked nevieme, co je novy bod, pozriem sa na jeho najblizsich susedov v treningovych datach  
Zvolime "dato" na zaklade `k` najblizsich susedoch  
Napriklad si zvolime ze $k = 2$, cize mozeme robit priemer 2 najblizsich susedov
**Hodnota `k` sa voli na validacnych datach**

Problem - pri zvoleni susedov moze byt problem s mierkami (scale)  
Napr. x-ova os moze vyjadrovat pocet izieb (2-5), y-ova os moze vyjadrovat rozlohu (40-150)  
Mozu byt vyrazne odlisne vysledky

Napr. pre byt $62 m^2$ a 3 izby

![knn1](../images/pans-knn1.png)  
![knn2](../images/pans-knn2.png)

Pri prvom nam vyjde cena 180 tisic, pri druhom 162 tisic, a z linearnej regresie nam vyslo 174 tisic

Riesenie

- Stanardizovat $x_1$ a $x_2$ - odcitanie priemeru a delenie smerodajnou odchylkou
- Normalizacia - scale na interval $<0; 1>$

#### k-means

`k`-priemery

Priradime "dato" k nejakemu centroidu

Zvolime si `k` centroidov, nasledne robime v cykle:

- Kazde "dato" priradim k najblizsiemu centroidu (priamka medzi nimi, v strede kolmica)
- Pre kazdu skupinu dat priradenych k centroidu vypocitam tazisko
- Posuniem centroid na vypocitane tazisko

### Co ak mame vela priznakov

Linearna regresia je super ked mame malo (a kvalitnych) priznakov - rozloha bytu a pocet izieb  
Problem vznika ked mame vela parametrov, napr. [MNIST dataset](https://github.com/cvdfoundation/mnist) - obrazky 28x28 pixelov = 784 priznakov  
Tu sa straca interpretovatelnost - nevieme povedat co konkretne vyjadruje jeden dany pixel  
Linearna regresia je interpretovatelna iba ak mame malo priznakov a kvalitne priznaky

Riesenie

- PCA - neda sa pouzit vzdy
- Embeddingy (?)
- **Deep Learning** (DL) - automaticky zisti priznaky (_features_) - **neuronove siete**

Preco Deep Learning?

- Pri klasickom ML (machine learning)
  - Dobre priznaky
  - Jednoduchy model
  - Vyhoda - interpretovatelnost (ak je priznakov malo)
- Neuronove siete - Deep Learning
  - Vrstvy postupne transformuju vstup na reprezentaciu
  - Z reprezentacie sa robi predikcia/klasifikacia
  - Nevyhoda - casto nie je interpretovatelne - pri MNIST nevieme popisat co robi konkretny pixel

Extrakcia priznakov je casto klucova - a DL je cesta

## Zaklady hlbokeho strojoveho ucenia

### Neuronova siet

Neuronova siet = funkcia

- Zoberie vektor s vela komponentami (28x28 = 784 rozmerny vektor)
- Vrati nam nejaku hodnotu (napr. ci je obrazok cislo 5)

**Proces**: Realita - Kvantifikacia - Model

- Oblacnost, unava - Hotnoty $x, y$ - Funkcia $z = f(x, y)$
- Obrazok 28x28 pixelov - Vektor s 784 komponentami - Funkcia $z = f(\mathbf{x})$, kde $\mathbf{x}$ je vektor, funkcia vracia cislo $\in <0 .. 1>$

Priklad s festivalom

- Uplne jednoducho - dcera chce ist ak su aspon 2 podmienky splnene
  - $x_1 + x_2 + x_3 > 1$
- Pridame vahy - zvazi to viac ak s nou pojde priatel
  - $0.2x_1 + 1.8x_2 + 1x_3 > 1$
- Vo vseobecnosti
  - $w_1x_1 + w_2x_2 + w_3x_3 > T$

Parametre $x_1$, $x_2$, $x_3$ - do vektora $\mathbf{x}$  
Vahy $w_1, w_2, w_3$ - do vektora $\mathbf{w}$  
Bias $b$  
Ci ideme je funkcia $\sum_{i=1}^{n} w_i x_i + b >= 0$, alebo vektorovo $\mathbf{w} \cdot \mathbf{x} + b >= 0$

Ak predchadzajuca funkcia $>=0$ tak rozhodnutie je $1$, inak je rozhodnutie $0$
Pouzijem **Heaviside-ovu funkciu** - pre hodnoty $=< 0$ dava $0$, pre hodnoty $> 0$ dava $0$

![heaviside funkcia](../images/pans-heaviside.png)

Matematicky zapis $y = f(\mathbf{x}) = H(\mathbf{w} \cdot \mathbf{x} + b)$  
Funkcia $H(z)$ (Heaviside-ova funkcia) sa nazyva aktivacna funkcia

Ucenie vahy ($\mathbf{w}$) a biasu ($\mathbf{b}$) = parametrov (alebo hyperparametrov) = **proces ucenia sa modelu**  
Hladanie parametrov tak, aby model sedel na data

Do neuronu dame vstupy s vahami, neuron da nam vystup

### Perceptron

Typ neuronu  
Je aktivovany Heaviside-ovou funkciou

- `0` ak $\mathbf{w} \cdot \mathbf{x} + b <= 0$
- `1` ak $\mathbf{w} \cdot \mathbf{x} + b > 0$

**Perceptron** - typ neuronu, ktory pouziva aktivacnu funkciu `H` (Heaviside)

![perceptron](../images/pans-perceptron1.png)

Viac perceptronov

- Napr. priklad s festivalom - nemame iba dceru ale aj dalsich 2 synov
- Neprehladne ked mame specificky vyberat ze ktory vstup do ktoreho perceptronu
- Riesenie - kazdy vstup do kazdeho perceptronu a menime vahy, tam kde nechceme nic dame vahu 0
- Vahy $\mathbf{w}_{ij}$ - z parametru $i$ do percepronu $j$
- Biasy $b_j$, kde $j \in (1, 2, 3)$

Parametre = vahy + biasy

![perceptrony](../images/pans-perceptron2.png)

Rozhodneme sa ze na festival pojdeme ak chcu ist aspon dvaja

![perceptrony](../images/pans-perceptron3.png)

#### Problem nespojitosti

Mala zmena ma velky dopad na vysledok (prekrocenie thresholdu)

> Napr. pocasie za zmeni o 1 stupen tak sa zrazu nejde na festival

Tento problem vyplyva zo skoku vo funkcii $H$

Riesenie - pouzitie inej aktivacnej funkcie

### Aktivacne funkcie

#### Heaviside-ova funkcia

Uz sa spominala predchvilou  
Perceptron

![heaviside](../images/pans-heaviside.png)

#### Sigmoidova funkcia

Taka vlnka

- Ak je vystup velmi zaporny $\rightarrow$ `0`
- Ak je vystup velmi kladny $\rightarrow$ `1`
- Ak je vystup niekde medzi, tak vystup bude niekde medzi `0` a `1`

Vzorec: $\sigma(z) = \dfrac{1}{1 + e^{-z}}$

![sigmoid](../images/pans-sigmoid.png)

#### ReLU

Rectified Linear Unit

- Ak $vystup < 0 \rightarrow 0$
- Ak $vystup \ge 0 \rightarrow <1 .. \infty >$ (nad $1$ rastie linearne)

$max(0, z)$

![relu](../images/pans-relu.png)

### Terminologia

- Vstupna vrstva
- Skryte vrstvy
- Vystupna vrstva
- Aktivacna vrstva
- Neuron
- Vahy
- Biasy
- Vrstva
- Sirka - pocet neuronov v jednej vrstve
- Hlbka - pocet vrstiev
- Parametre - vahy + biasy

![siet](../images/pans-siet1.png)

### Formalizacia ucenia sa neuronovej siete

Pome to dat nejak dokopy  
Priklad s festivalom

- 6 vstupov - pocasie, mhd, kamarati, ...
- 3 deti - rozhodnu sa na zaklade vstupov
- 2 rodicia - rozhodnu sa na zaklade deti
- 1 vystup - rozhodne sa podla toho co povedia rodicia

**Siet je funkcia**

- Zoberie vstupy $x_1, ..., x_6$
- Vrati nam vystup - hodnotu, cislo $\in <0..1>$
- Ma vnutri parametre $\mathbf{w}$ a $\mathbf{b}$
- K dispozicii ma mnoho dvojic $\mathbf{x}, y$

Ciel - najst take $\mathbf{w}$ a $\mathbf{b}$ aby predikovane hodnoty $\hat{y}$ boli co najblizsie $y$  
$\hat{y} = F_{w, b}(x)$ sa ma podobat na $y$ = minimalizacia MSE (mean square error) - rozdiely na druhu  
Minimalizujeme $MSE(\mathbf{w}, \mathbf{b}) = \dfrac{1}{n} \sum_{data}(\hat{y} - y)^2 = \dfrac{1}{n} \sum_{data}(F_{w, b}(x) - y)^2$  
Vstupy $x$, data $y$, predikcia $\hat{y} = F_{w, b}(x)$  
V podstate metoda najmensich stvorcov z AP  
$min(MSE(w,b))$

#### Minimalizacia $MSE(\mathbf{w}, \mathbf{b})$

Pri jednom parametri - jediny neuron - hladanie minima funkcie = miesto kde $derivacia = 0$  
Pri viacerych premennych - $parcialne derivacie = 0$  
Pri 784 premennych je to fess problem - sustava 784 rovnic

Riesenie - **iterovanie**

- Som v horach, strasna hmla, nevidim na 2 metre, snazim sa dostat dole
- Iterujem
  - Stojim na mieste, pootacam sa dookola, najdem _smer najvacsieho sklonu_ - kolmo na vrstevnicu
  - Spravim par krokov dopredu
- $(x_{n+1}, y_{n+1}) = (x_n, y_n) - \eta \cdot (Dx, Dy)$
- $n = n+1$
- Moze sa stat ze nepridem dole na parkovisko ku autu, ale niekde do ineho udolia/jazierka/cojaviemco

$(w_{n+1}, b_{n+1}) = (w_n, b_n) - \eta \cdot (dF/dw, dF/db)$  
Klucovy krok su parcialne derivacie $F$

Iterovanie (derivacie) nemusia fungovat pri perceptrone (Heavisideova funkcia)

#### Learning rate

Dlzka kroku ked idem dole z kopca

- Moze sa stat ze je moc maly - moc dlho pojdeme - dlho to trva
- Moze sa stat ze je moc velky - mozeme prepasnut to minimum a nikdy minimum nenajdeme
