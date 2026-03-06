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

## Trening neuronovej siete - od chyby k vahe

### Data, parametre, hyperparametre

Supervised learning pracuje s parmi `(X, Y)`

- `X` - priznaky (features) - co vieme zmerat o kazdom priklade - plocha bytu, pocet izieb, ...
- `Y` - label (stitok/anotacia) - co chceme predpovedat - cena bytu

V buducnosti chcem co najpresnejsie predikovat `X`, ktore momentalne nepozna

#### Parametre vs. Hyperparametre

Parametre

- Hodnoty, ktore sa bude siet ucit
- Napr. Vahy, Biasy
- Nastavia sa pocas trenovania - optimalizacna metoda ich aktualizuje automaticky v kazdom krokou (gradient descent)

Hyperparametre

- Nastavenia, ktore volime mey - pred treningom, alebo mimo nich
- Napr. Learning rate, pocet vrstiev, aktivacna funkcia
- Nastavia sa mimo treningu, optimalne hodnoty hladame na validacnych datach

Priklad

| Uloha             | Hyperparameter                                                              | Parameter                                      |
| ----------------- | --------------------------------------------------------------------------- | ---------------------------------------------- |
| k-NN              | `k` = pocet susedov, nastavovali sme na validacnych datach                  | Ziadne (k-NN si nic netrenuje)                 |
| Linearna regresia | Ziadne (zadavame iba data)                                                  | Koeficienty $w_1, w_2, b$ (najdene analyticky) |
| Neuronova siet    | Learning rate $\eta$, pocet vrstiev, pocet neuronov, aktivacna funkcia, ... | Vahy a biasy - $w, b$                          |

### Stratova funkcia a minimalizacia (optimalizacia)

Treningovy cyklus (toto iterujeme)

1. Data `(X, Y)`
2. Forward pass - predikcie siete $\hat{y} = f(X, w)$ - vypocty
3. Loss - $L(\hat{y}, y)$ - zmeranie chyby, napr. MSE
4. Gradient - $\partial L / \partial w$ - **backpropagation**
5. Aktualizacia vah - optimalizacia

#### Vystupna vrstva zavisi od ulohy

- Regresia
- Binarna klasifikacia
- Multi-class klasifikacia

|                   | Regresia                                  | Binarna klasifikacia                  | Multi-class                        |
| ----------------- | ----------------------------------------- | ------------------------------------- | ---------------------------------- |
| Pocet vystupov    | 1 vystupny neuron                         | 1 vystupny neuron                     | $N$ vystupnych neuronov (1/trieda) |
| Aktivacna funkcia | Bez aktivacnej funkcie                    | Sigmoid $\sigma (z)$                  | Softmax                            |
| Vystupny rozsah   | Vystup $\in \mathbb{R}$ - lubovolne cislo | Vystup $\in (0, 1)$ - pravdepodobnost | Vektor pravdepodobnosti, sucet = 1 |
| Priklad           | Cena bytu, predikcia teploty              | Spam/nie spam, chory/zdravy           | Pes, macka, vtak, cifry 0-9        |

#### Linearna regresia

$\hat{y} = w \cdot x + b$

Najjednoduchsi priklad

> Aktivacna funkcia prinasa nelinearitu, my chceme _linearnu_ regresiu

Treningovy cyklus

1. Vypocet forward pass - $\hat{y} = w \cdot x + b$
2. Vypocet chyby - Loss $L = (y - \hat{y})^2$
3. Gradient - parcialne derivacie - $\eta L / \partial w$
4. Update parametrov - $w \leftarrow w - \eta \cdot \partial L / \partial w$

#### Stratova funkcia

Stratova funkcia $\dfrac{1}{n} \sum (\hat{y_i} - y_i)^2$ - MSE (mean squared error)  
Potrebujeme zisti ci siet predikuje dobre - potrebujeme jedno cislo - stratu

- Na druhu lebo nech sa to nevykompenzuje (kladne + zaporne), cize vzdy bude kladne
- Vacsia chyba = vacsi dopad, su penalizovane viac
- $\dfrac{1}{n}$ lebo priemer, $n$ je pocet dat alebo coho to idk, proste priemer

#### Minimalizacia straty - 1D

MSE je ako funkcia vahy $w$, teda $L(w)$ je krivka, ktorej minimum hladame  
Kde je minimum? Tam kde je derivacia = 0

Gradient descent ($\partial L / \partial w$) hovori o tom, ktorym smerom strata rastie, ked zvysime $w$

- Ak je gradient kladny - strata rastie smerom doprava - my musime ist dolava
- Ak je gradient zaporny - strata rastie smerom dolava - my musime ist doprava

**Vypocitame sklon krivky v aktualnom bode a ideme opacnym smerom**

$w \leftarrow w - \eta \cdot \partial L / \partial w$

$\partial L / \partial w > 0$ = krivka stupa - $w$ zmensime
$\partial L / \partial w <= 0$ = krivka klesa - $w$ zvacsime

#### Minimalizacia straty - 2D a mnoho D

Mame 2 vahy - $w_1, w_2$, MSE je plocha

$\nabla L = [\partial L / \partial w_1, \partial L / \partial w_2]$

Zvlast pocitame pre kazdu vahu  
Kazdu vahu potom updatneme zvlast

- $w_1 \leftarrow w_1 - \eta \cdot \partial L / \partial w_1$
- $w_2 \leftarrow w_2 - \eta \cdot \partial L / \partial w_2$

Pri vyssich dimenziach ako 2 je to to iste, len s viac vahami

#### Learning rate $\eta$ - velkost kroku

Ak prilis maly - pomala konvergencia (trva dlho), mozeme uviaznut v lokalnom minime  
Ak prilis velky - preskakujeme minimum, divergencia, loss stupa namiesto klesania  
"Optimalny" - plynula konvergencia, uplne optimalny neexistuje

Learning rate $\eta$ je hyperparameter, volime ho my, byva to male cislo ($\approx 0.001$), este sa k tomu dostaneme

Kazdy krok je narocne vypocitat - forward pass, derivacie, ...

### Backpropagation

Ako zistit, ktoru vahu zmenit a o kolko

Siet ma miliony/biliony vah  
Po forward passe vieme stratu $L$  
Ako zistit konkretne ktoru vahu zmenit a o kolko

- Nahodne zmeny - mozno ok pre mensie siete, pri vacsich nepouzitelne
- Numericky gradient
  - Pre kazdu vahu $w$ spocitame $(L(w + \epsilon) - L(w)) / \epsilon$
  - Je to presne, ale pri 1M vahach je to 1M doprednych krokov
- Metaheuristiky? - trochu "prehladame" miesto kde dostavame dobre vysledky, ten isty problem co pri nahodnych zmenach
- Backpropagation
  - Analyticky gradient pomocou **Chain rule**
  - Siet je velka funkcia - $a ( a ( a (w \cdot x + b)))$, kde $a$ je aktivacna funkcia
  - 1 spatny prechod
  - $\partial L / \partial w$ pre kazdu vahu naraz, rovnaka presnost

Backward pass = backpropagation + gradient descent

#### Preco nie numericky gradient

Pri numerickom gradiente je takyto postup

1. Vyber vahu $w_1$ (z tisicov vah)
2. Spusti forward pass s $w_1 + \epsilon$ - cely dataset
3. Spocitaj gradient pre $w_1$ - derivacia
4. Opakuj pre $w_2, w_3, ... w_n$, kde $n$ = pocet vah (tisice/miliony)

Pri backprop sa vyuziva chain rule - $\partial L / \partial w = \partial L / \partial a \cdot \partial a / \partial z \cdot \partial z / \partial w$

1. Pri forward passe ulozime medzivysledky
2. Spocitaj $\partial L / \partial \hat{y}$ pri vystupe - zaciatok spatneho prechodu
3. Sir gradienty dozadu (chain rule) - vrstva po vrstve
4. Vysledok = $\partial L / \partial w$ pre kazdu vahu - vsetky naraz, jeden prechod

Tym padom pri numerickom gradiente mame tisice/miliony/miliardy forward passow pre kazdy krok treningu  
Pri backprop mame jeden forward + jeden backward na kazdy krok

Backprop vyuziva medzivysledky z forward passu - nepotrebuje opakovat dopredny prechod pre kazdu vahu

#### Backdrop $\ne$ Gradient descent

Backprop

- Vypocita gradienty $\partial L / \partial w$ pre kazdu vahu
- Nic nemeni, len pocita
- Vystup je vektor gradientov - $[\partial L / \partial w_1, \partial L / \partial w_2, ...]$

Optimizator - napr. gradient descent

- Vezme gradienty a aktualizuje na ich zaklade vahy
- $w \leftarrow w - \eta \cdot \partial L / \partial w$
- Su aj ine optimizatory - Adam, RMSProp - tiez vyuzivaju gradienty, aktualizuju inak

Backprop pocita gradienty, optimizator s nimi actually nieco robi

**trening = backdrop + optimizator + data**

#### Chain rule

Chain rule je zaklad backprop

Strata $L$ zavisi od aktivacie $a$  
Aktivacia $a$ zavisi od $z$  
$z$ zavisi od $w$

Otazka - o kolko zmena $w$ ovplyvni $L$

![chainrule](../images/pans-chain-rule.png)

Normalne pri forward pass ideme takto

1. Vaha $w$
2. Linearna kombinacia $z = w \cdot x + b$
3. Aktivacia $a = \delta (x)$
4. Strata $L(a, y)$

Tuto ideme odkonca, dobime derivacie

1. Vieme stratu, spravime $\partial L / \partial a$
2. $\partial a / \partial z$
3. $\partial z / \partial w$

$\partial L / \partial w = \partial L / \partial a \cdot \partial a / \partial z \cdot \partial z / \partial w$

Treba sa pozerat na derivacie  
Derivacie aktivacnych funkcii, derivacie neviem coho  
Staci vediet 1. derivaciu  
System musi mat derivaciu

Backprop zvycajne nenajde globalne minimum  
Najde lokalne alebo uviazne v sedlovom bode  
Pre hlboke siete sedlove body dominuju nad lokalnymi minimami  
V praxi to vacsinou nevadi

Backprop nemeni architekturu siete, iba hodnoty vah a biasov  
Pocet vrstiev a neuronov je fixny pocas celeho treningu

Velkost kroku zavisi od $\eta$ aj $| \partial L / \partial w |$ (hyperparameter aj strmost povrchu)  
Blizsie k minimu su kroky prirodzene mensie

Nie kazdy forward pass musi vidiet cely dataset, v buducnosti budeme pouzivat mini-batche

### Problemy pri treningu siete

#### Vanishing gradient

Sigmoid

- Dobre vlastnosti
  - V strede spravi mala zmena velky rozdiel - rozhodnost
- Neprijemne vlastnosti
  - Uplne "na zaciatku a na konci" sa "hybe pomaly" - saturovany
    - Velmi to spomali trening pri tychto hodnotach
  - Derivacia sigmoidu = max 0.25
    - V kazdom kroku sa zmeni minimalne o 0.25 nasobok
    - Nasobime max. 0.25
    - Po viac vrstvach sa moze stat ze vysledok bude 0
    - Prve vrstvy su takmer bez gradientu - prestanu sa ucit

ReLU - rectified linear unit

- Derivacia 0 alebo 1
  - V bode 0 actually nema ale tvarime sa ze ma - dame tam proste ze je bud 0 alebo 1
- Cize sa ten trening nespomali ako pri sigmoide
- Nevyhoda - pri 0 sa neuron neuci, "da sa z toho nejak dostat ale vy sa z toho nedostanete"
  - Riesenie - leaky relu, elu, ...

Vanishing gradiet = siet sa uci cim dalej tym pomalsie

#### Exploding gradient

Pri predchadzajucich problemoch bol problem s malymi vahami  
Preco nedrzat velke vahy?  
Vahy sa mozu zacat exponenscialne zvysovat, potom nepresnosti s pocitanim a dalsie obmedzenia

Riesienie - zamedzit _nieco_ >1

#### Preco ReLU
