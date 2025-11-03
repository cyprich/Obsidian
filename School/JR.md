# JR

Jazyk Rust  
Hodnotenie - semestralka 55, praca na cviceniach 5, prakticka skuska 40

## Keywords

Klucove slova

Aktualne

```rs
as
async
await
break
const
continue
crate
...
```

Rezervovane pre buduce pouzitie

```rs
abstract
become
...
```

Surove identifikatory - umoznuju pouzivat klucove slova ako identifikatory  
Predpona `r#`

```rs
fn r#match(...) { ... }
```

## Shadowing

```rs
let x = 5;
let x = 'a';
```

## Typovost

Silna staticka  
Nevieme obijst typovy system,

## Datove typy

Skalarne, Zlozene, Logicky typ (bool), Znakove typy (char)

Default cislo `i32`, poradie `usize`

```rs
std::i32::MAX  // maximalna hodnota i32
```

Defaultna ochrana pred pretecenim  
Da sa obist

```rs
let mut x: u8 = 0;
x = x.wrapping_sub();
```

```rs
fn main() {
  let b: bool = 0;  // error
  let b: bool = 0.into();
}
```

### Pole

Viacere hodnoty rovnakeho typu

```rs
let a = [1, 2, 3];
let a: [i32; 3] = [1, 2, 3];
let a = [3; 5];  // [3, 3, 3, 3, 3]

let x = a[0];
```

### N-tica (touple)

```rs
let t = (10, 5.5, 23);
let t: (i32, f64, u16) = (10, 5.5, 23);

let x = t.2;
```

### Struct

```rs
struct User {
  id: i32,
  username: String,
  email: String
}

let u = User {
  id: 0,
  username: String::from("jozko"),
  email: String::from("jozko@jozko.sk"),
}

let u2 = User {
  id: 1,
  ..u  // "zdedi" vsetko ostatne od 'u'
}
```

Struktura bud cela modifikovatelna alebo vobec  
Pozor na zmenu ownershipu

### N-ticove struktury

```rs
struct Point {x: i32, y: i32, x: i32};  // struktura
struct Point (i32, i32, i32);  // n-ticova struktura
```

### Enum

```rs
enum IP {
  v4,
  v6
}

enum IP {
  v4(String),
  v6(String)
}

let t = IP::v4(String::from("172.0.0.1"));
let t = IP::v6(String::from("::1"));

enum JednotkaVzdialenosti {
  km, m, dm, cm, mm
}

fn daj_vzdialenost_v_mm(vzdialenost: JednotkaVzdialenosti) -> i32 {
  match vzdialenost {
    JednotkaVzdialenosti::km => 1_000_000,
    JednotkaVzdialenosti::m => 1_000,
    ...  // treba dat vsetky mozne hodnoty
  }
}
```

## Ciselne literaly

```rs
let x = 12_345
let x = 0x1f
let x = 0o56
let x = 0b1111_0000
let x = b'A'
```

## Aritmeticke, logicke a bitove operacie

+, -, ...

`^` = XOR

## Match

```rs
match hodnota {
  6 => println!("hodnota je 6")
  nieco_ine => println!("hodnota nie je 6, ale {nieco_ine}")
}

match hodnota {
  6 => sprav_nieco(),
  _ => sprav_nieco_ine()
}
```

## Funkcie

```rs
fn nazov_funkcie_v_snake_case(parameter: String) -> navratova_hodnota { ... }
```

## Metody

```rs
struct User {
  id: i32,
  username: String
}

impl User {
  fn vypis_pouzivatela(&self) {
    println!("Pouzivatel {self.username}, id: {self.id}");
  }
}

let u = User { ... }
u.vypis_pouzivatela();
```

Moze byt viac `impl` blokov, na roznych miestach  
Mozem spravit `impl` blok pre kod z inej kniznice  
Daju sa robit aj staticke - `String::from()` - _asociovana funkcia_, tu sa pouziva `::`, nie `.`

## Vetvenie

```rs
if condition {
  ...
} else {
  ...
}

let cislo = if true { 6 } else { 7 };

// musia vraciat rovnaky typ
let nieco = if true { 6 } else { "Hello" };  // error
```

## Cykly

loop, while, for, break, continue

Volania `break` a `continue` sa by default aplikuju na najvnutornejsi loop  
Ak mame viac nested loops, mozeme jednotlive pomenovat (`'vonkajsi: loop { ... }`) a potom breaknut vybrany loop (`break 'vonkajsi`)

## Vlastnictvo a poziciavanie

Pravidla

- Kazda hodnota ma vlastnika
- V jednoma case moze byt len jeden vlastnik
- Ked vlastnik odide z bloku, hodnota sa zahodi

`Copy` trait - operacia Move namiesto kopirovania

## Zivotnost premennych a casove anotacie

Pristup k hodnote po jej uvolneni, ked nezije dostatocne dlho  
Borrow checker porovnava rozsahy platnosti premennych

Explicitne casove anotacie

```rs
&i32   // referencia
&'a i32  // referencia s explicitnou casovou anotaciou

```

Napr...

```rs
fn vrat_dlhsi_retazec<'a>(x: &'a str, y: &'a str) => &'a str {
  if ...
}
```

Platnost premennej zostava taka ista, iba popisuju vztah zivostnoti viacerych referencii

## Moduly

Oranizacia kodu do bednicky (crate)  
Riadenie viditelnosti - by default pristupne len vramci modulu

`mod` - zaciatok modulu  
`use` - vytvorenie skratky k ceste modulu - basically nieco ako import  
`pub` - public modifier

```rs
pub mod vonkajsi_modul {
  fn moja_funkcia() { ... }

  mod vnutorny_modul {
    pub fn moja_druha_funkcia() { ... }
  }
}

use crate::vonkajsi_modul::moja_funkcia;
use crate::vonkajsi_modul::vnutorny_modul::moja_druha_funkcia;

fn main() { ... }

// nebudeme moct pristupit ani k jednemu, lebo aj pri jednom aj pri druhom je nieco private
```

## Kolekcie

### Vektor `Vec`

```rs
let v: Vec<i32> = Vec::new();
let mut v = vec![1, 2, 3];
v.push(5);
let x: &i32 = &v[0];  // ak netrafime, tak spanikari
let x: Option<&i32> = v.get(3);
```

### Hesovacia Tabulka `HashMap`

HashMap (algoritmus SipHash1-3, odolny ovci HashDoS, nadhodne seedovany zo systemu), ...

```rs
let mut h = HashMap::new();
let mut h =

#[derive(PartialEq, Eq, Hash)]
struct VlastnyKluc {
  a: i32,
  b: String
}
```

### Dalsie

VecDeque, LinkedList, BTreeMap, HashSet (HashMap, kde typ je `()`), BinaryHeap

## Retazce

Najcastejsie sa pouzivaju 2

- `&str`
  - Vlastne `&[u8]`
  - Nevlastni retazec, ale len nan ukazuje
- `String`
  - Na halde
  - Implemetovany ako `vec<u8>`, ale garantuje, ze je vzdy platna UTF-8 sekvencia

Na UTF-8 sa da pozriet troma sposobmi

- .
- .
- .

## Modul `env`

Environment variables

```rs
for i in std::env::args() { ... }

for (key, value) in std::env::vars() { ... }
```

## Chyby a spracovanie vynimiek

Chyby mozu byt

- Obnovitelne - nechceme aby ukoncili program - bezne mozu nastat - vstup od pouzivatela (neplatny vstup, chybna cesta k suboru) - enum `Result<T, E>`
- Neobnovitelne - makro `panic!()` - neplatny index v poli, ...

Pri pouziti `panic!` sa deje toto

1. Vypise sa chybova hlaska
2. Spusti sa proces odvijania zasobnika
3. Vycisti sa zasobnik
4. Ukonci sa program

Spravanie sa da upravit v `env` alebo v `Cargo.toml`

```toml
[profile.release]
panic = 'abort'
```

## Funkcionalne programovanie

Umoznuje uchovavat anonymne funkcie v premennych  
Closures - funkcne uzavery

```rs
fn fun1(a: i32, b: i32) -> i32 { a + b }

let fun2 = | a: i32, b: i32 | -> i32 { a + b };
let fun3 = | a, b | { a + b };
let fun4 = | a, b | a + b;
```

```rs
let funkcia = |x| x;
let string = funkcia(String::from("hello"));
let cislo = 10;  // error here - zmenime typ - implicitne je string podla riadku nad tymto
```

## Iteratory

Umoznuju vykonat ulohu nad sekvenciou hodnot

```rs
pub trait Iterator {
  type Item;
  fn next(&mut self) -> Option<Self::Item>;
}
```

Ma aj dalsie funkcie, rozdelene su do 2 kategorii

- Konzumne adaptery - spotrebuvavaju iterator, interne volaju next, po volani uz nemozeme dalej pouzivat iterator - napr. `sum`
- Iteratorove adaptery - nespotrebuvavaju iterator, ale vytvara novy iterator na zaklade povodneho - napr. `map` vykona modifikaciu nad kazdym prvkom

```rs
let v = vec![1, 2, 3];
let i = v.iter();

let v2 = i.map(|x| x+1);  // lazy vykonavanie - vrati `Map { iter: Iter([2, 3, 4]) }`
let v3 = i.map(|x| x+1).collect();  // vrati vektor

let i = 2;
let vacsie_ako_i = v.iter().filter(|s| s>i).collect();
```

Rust vyuziva _"zero-cost abstractions"_ - kod je rovnako rychly s aj bez pouzitia iteratorov

## Genericke programovanie

```rs
fn funkcia<T>(parameter: &T) -> &T {
  parameter
}

struct Pair<T> {
  x: T,
  y: T
}

let p1 = Pair { x: 10, y: 12 };
let p1 = Pair { x: 10.5, y: 12.5 };
let p1 = Pair { x: 10, y: 12.5 };  // error here

impl<T> Pair<T> {
  fn daj_x(&self) -> i32 { self.x }
}

// alebo definujem funkciu iba pre typ i32
impl Pair<i32> {
  fn daj_x(&self) -> i32 { self.x }
}
```

Mozem mat aj viac parametrov  
Mozem ich mixovat

Neovplyvnuje rychlost - _"monomorfizacia kodu"_ - vyhodnotene uz pocas kompilacie, generuje funkcie pre kazdy pouzity typ

## Trait

Umoznuju definovat podobne spravanie pre viacero typov  
Podobne ako `interface`, ale nie rovnake -
Moze byt aj pre primitivne typy _(?)_

```rs
trait VypisSa {
  fn vypis(&self) -> String;
}

trait VypisSa {
  fn vypis(&self) -> String {
    String::from("default hodnota")
  }
}

struct S { ... }

impl VypisSa for S {
  fn vypis(&self) -> String { ... }
}

```

Pouzivanie premennych ktore implementuju dany trait

```rs
fn funk(param: &impl VypisSa) {
  println!("{}", param.VypisSa())
}

// to iste sa da zapisat takto
fn funk<T: VypisSa>(param: T) {
  println!("{}", param.VypisSa())
}
```

Implementuje viac traitov

```rs
fn funk(param: &(impl VypisSa + Display)) {
  println!("{}", param.VypisSa())
}

fun funkcia<T, K>(param: &T, param2: &K)
where
  T: VypisSa + Display,
  K: Display + Clone
{ ... }
```

Mozem pouzit ako navratovu hodnotu

```rs
fn funk() -> impl VypiSa { ... }
```

Plosne zavedenie traitu  
Mozeme volat napr. `10.to_string()`

```rs
trait <T: Display> ToString for T { ... }
```

Poskytnutie standardnej implementacie pomocou `derive`  
By default je mozne odvodit `Eq`, `PartialEq`, `Ord`, `PartialOrd`, `clone`, `Copy`, `Hash`, `Default`, `Debug`

```rs
#[derive(Debug)]
struct S { ... }
```

Da sa aj odvodit vlastne pomocou makra

## Staticky a dynamicky dispatch

Mechanizmus, ktory umoznuje volat funkcie - rozhoduje ktora funkcia sa vola pri volani funkcie

Staticky dispatch - vyhodnotene pri kompilacii - monomorfizacia  
Dynamicky dispatch - tabulka virtualnych metod - pocas behu = pomalsie - potrebne zadat klucove slovo `dyn`

## Kniznice

## Clap

Command Line Argument Parser

[Dokumentacia](https://docs.rs/clap/latest/clap)

## Serde

Serializacia, Deserializacia

## Crate

Bednicky  
Kompilacna jednotka v jazyku Rust  
Moze obsahovat moduly  
Vysledkom binarka alebo kniznica  
Kniznica = `--crate-type=lib` alebo v `Cargo.toml`

## Cargo

Pokrocilejsia konfiguracia projektu

Standardne profily `dev` a `release` (`cargo build --release`)

Dokumentacia `cargo doc --open`

Dokumentacne komentare

- `///`
- Znacky zacinajuce `#` - Examples, Panic, ...
- `//!` na popis modulu (nie jeho jednotlivych prvkov)

Kniznica by mala byt jednoducho pouzitelna  
`pub use` - zoberie viditelnu vec z jedneho miesta a spravi ju viditelnou na inom mieste, ako keby bola definovana tam

Upload do _crates.io_ pomocou `cargo publish`, ale treba tam toho viac

Organizacia projektov pomocou `workspace`  
Viacero crates, ktore zdielaju jeden `Cargo.lock` subor  
Namiesto `[package]` spravime `[workspace]`

```toml
[workspace]
resolver = "3"
```

Funkcie jednej crate v druhej crate

```toml
[package]
...

[dependencies]
moja_kniznica = { path = "../moja_kniznica" }
```

Testy jednej bednicky - `cargo test -p nazov_bednicky`

`cargo instal wthrr`

## Smart pointre

V Ruste sa casto stretavame so smernikom - referencie - `&`  
Smart pointre - okrem samotneho pointra aj dodatocne informacie

V Ruste najcastejsie 3 smart pointre

- `Box<T>`
- `Rc<T>`
- `RefCell<T>`

Mozeme si spravit vlastne smart pointre pomocou traitov `Deref` a `Drop`

### `Box<T>`

Alokovanie na halde namiesto na stacku  
Neovplyvnuje rychlost, nedodava dodatocnu funkcionalitu  
V C++ unique

Pouziva sa ked

- Pracujeme s
-
-

```rs
let b = Box::new(10);
println!("{b}");
```

Priklad

```rs
struct Osoba {
  meno: String,
  // otec: Option<Osoba>  // error - infinite recursion
  otec: Box<Option<Osoba>>
}

let main() {
  let o = Osoba { ... }
  let otec = *o.otec;  // actually nemusim pouzivat `*`, pretoze Rust ma auto dereferencie
}
```

Trait `Deref` - implicitna korekcia dereferencii  
Podobne `DerefMut` - menitelna referencia

Trait `Drop` - implicitne sa vola funkcia `fn drop (&mut self)` ked pointer odide z kontextu  
Explicitne sa `drop` neda zavolat  
Mozeme ale pomocou `std::mem::drop(premenna)`

### `Rc<T>`

Reference Couter  
V C++ shared  
Sleduje pocet referencii  
Ked pocet referencii padne na 0, tak sa uvolni  
`Rc::new()`, `Rc::clone()`, `Rc::strong_count()`  
Iba nemenitelne udaje!

### `RefCell<T>`

Vnutorne menitelny pattern nam umoznuje menit udaje aj ked mame na ne nemenitelnu referenciu  
Pattern je implementovany v `unsafe` bloku, je za nejakym safe rozhranim

Zabezpecuje jedineho vlastnika (ako `Box`), ale pravidla poziciavania su kontrolovane pocas behu (namiesto casu kompilacie)

Vyhody kontroly poziciavania pocas behu

- Dovoluje riesit nejake specificke situacie, kedy sa nieco neda overit pocas kompilacie
-

Mozeme pouzivat ked my sme si isti, ale kompilator nie

`RefCell::new`, `RefCell::borrow`, `RefCell::borrow_mut`,

Ked skombinujeme `Rc` a `RefCell`, mozeme mat viac vlastnikov menitelnych dat

### Silne a slabe referencie

Pomocou `Rc` a `RefCell` by sme mohli spravit cyklicke referencie medzi objektami  
Keby obidva objekty odidu z kontextu, pocitadlo referencii by nebolo `0`  
Tu by sme mali pouzit `Weak<T>` namiesto `Rc<T>`, ktory nevlastni udaje  
Switch medzi nimi pomocou funkcii `downgrade`, resp. `upgrade`

## Testovanie

Je mozne vyuzit specialne funkcie ktore obsahuju atribut `#[test]`  
`cargo test` - vytvori a spusti specialne binarky funkcii ktore su oznacene ako `#[test]`

```rs
pub fn add(...) { ... }

#[cfg(test)]
mod test {
  use super::*;
  #[test]
  fn it_works(...) { ... }
}
```

Konkretne tu sa pouziva makro `assert_eq!`, ktore porovnava ci 2 veci sa rovnaju  
Dalsie makro je `assert!` - ci logicky vyraz nadobuda `true`, iba jeden argument  
Dalsie makro je `assert_ne!` - 2 hodnoty sa nemaju rovnat  
Tieto makra maju dalsi parameter - sprava ktora sa vypise v pripade zlyhania  
Ak niektore z tychto vrati false, tak sa vyvola `panic!`

Mozeme dat este atribut `#[should_panic]` ak kod ma vyvolat paniku

Ak sa nam nepaci panic, mozeme pracovat s `Result<K, T>`  
Tu ale nevolat should panic ani asserty

Pokrocilejsie nastavenie testovania - `cargo test --help`  
Nastavenia formatovania, poctu threadov, zobrazit vypisy, spustit aj s ignorovanymi, sputit iba ignorovane,

2 typy testov

- Jednotkove testy - vlastny modul s oznacenim `#[cfg(test)]` (tak ako ked vytvorime lib)
- Integracne testy - adresar `tests`, netreba `cfg`, kazdy subor je povazovany za crate, mozeme testovat len libraries (nie binarky)

## Asynchronne programovanie

Vykonavanie kodu, zatial co sa vykonava iny kod

V sucasnosti 2 techniky

- Paralelizmus - ulohy sa vykonavaju naraz v jednom momente
- Subeznost - ulohy sa vykonavaju na striedacku, ale iba jedna v jednom momente

Funkcie su 2 typov

- Blokujuce - zastavi program kym sa funkcia nedokonci - najcastejsie praca so subormi, so sietou, ...
- Neblokujuce - mozeme pocas diania robit dalsie veci

V Ruste sa pouziva `Future` - hodnota este nemusi byt k dispozicii, ale niekedy v buducnosti bude  
`Future` je trait  
Oznacenie funkcie `async`, v nej mozeme volat `await`  
Konktrola, ci je hodnota uz dostupna - polling

```rs
async fn asynch_funkcia() {
  let result = asynch_volanie().await;
}
```

Future je lazy - nic sa nevykona az do await

Async moze preberat vlastnictvo pomocou `move`

```rs

```

Problem pri poziciavani -

Problem pri spustani - nemozeme volat async funkciu v normalnej funkcii, main nemoze byt async  
Riesenie - asynchronny runtime - napr. `Tokio` - najviac pouzivany  
`cargo add tokio --features rt`

```rs
fn main() {
  let runtime = tokio::runtime::Bulder::new_current_thread().build().unwrap();
  runtime.block_on(async {
    asynchronna_funkcia().await
  })
}
```

Ak chceme mat main async  
`cargo add tokio --features "rt,macros,rt-multi-thread"`

```rs
#[tokio::main]
async fn main() { ... }
```

Ak chceme paralelne mozeme pouzit tasky

```rs
tokio::spawn(async {
  prva_funkcia().await
});
tokio::spawn(async {
  druha_funkcia().await
});

// alebo jednoduchsie

tokio::spawn(prva_funkcia());
tokio::spawn(druha_funkcia());
```

Ak chceme pockat na dokoncenie nejakych uloh pomocou makra `join!`

```rs
tokio::join!(
  tokio::spawn(fn1()),
  tokio::spawn(fn2())
);

println!("Skoncil som");
```

Pripad - nemame pristupne vsetky udaje, ale uz chcem s nimi pracovat - sledovanie videa online  
V Ruste `stream` - velmi podobne ako iteratory, ale asynchronne  
Kniznica `futures`

### Kanaly

Ked chcem vo viacerych vlaknach pouzivat rovnake zdroje  
Normalne by tam bol problem s lifetimes ci co

`tokio` implementuje viac modelov pri praci s kanalmi

- `mpsc` - multi-producer single-consumer kanar
- `oneshot` - single-producer single-consumer
- `broadcast`- multi-producer multi-consumer
- `watch`- multi-producer mutli-consumer

```rs
use ...

enum Command { ... } // enum pre Get a Set

#[tokio::main]
async fn main() {
  let (tx, mut rx) = mpsc::channel(32);

  let tx2 = tx.clone();

  tokio::spawn(async move {
    tx.send(...)
  });

  tokio::spawn(async move {
    tx2.send(...)
  });
}
```

### Trait `Future` a typ `Poll`

### `std::pin::Pin` a `Unpin`

Problem pri self-reference structs, lebo adresa referencie sa moze menit  
Riesenie - `std::pin::pin!` - premenna sa zarucene nemoze v pamati presunut

`Unpin` netreba implementovat

### Trait `Stream`

Typy, s ktorymi chceme pracovat ako s "async iteratormi" implementuju train `Stream`

## Asociovane typy

```rs
pub trait Stream {
  type Item ...  // toto je asociovany typ
}
```

## Fantomove typove parametre

Nepouzivaju sa pocas behu programu, ale kontroluju sa staticky pocas kompilacie  
Pouzivaju sa len ako znacky alebo kvoli kontrole typovosti  
Neuchovavaju ziadnu hodnotu a nemaju vplyv na beh programu
