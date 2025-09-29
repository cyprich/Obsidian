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
