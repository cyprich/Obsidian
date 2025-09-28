# Rust

Notes on the Rust Programming Language, following the [Rust Book](https://doc.rust-lang.org/book/)

## Installation

We will be using `rustup`  
You can follow [all installation methods](https://forge.rust-lang.org/infra/other-installation-methods.html)  
You can verify installation with `rustup --version` and `rustc --version`

You should also install `cargo` if not using the official installer  
Verify with `cargo --version`

You might also want to install `rust-analyzer`

## Hello World

```rs
fn main() {
  println!("hello world");
}
```

To run this code...

```bash
rustc helloworld.rs
./helloworld
```

## Cargo

Cargo is Rust's build system and package manager  
It makes life easier when working with external dependencies/libraries, known as **crates**  
You can check available crates at [crates.io](https://crates.io/)

There is a list of some of Cargo most used commands

| Command                  | Description                                               |
| ------------------------ | --------------------------------------------------------- |
| `cargo new project_name` | Creates new project                                       |
| `cargo build`            | Builds project                                            |
| `cargo build --release`  | Builds project for release                                |
| `cargo run`              | Builds and runs project                                   |
| `cargo check`            | Makes sure you code compiles, but doesn't make executable |
| `cargo add crate_name`   | Adds crate to project                                     |
| `cargo update`           | Updates all crates to newest versions                     |

## Guessing game

Create new project with `cargo new guessing_game`

Getting user input works like this...

```rs
use std::io;

fn main() {
  println!("Guessing game!");
  println!("Enter your guess: ");

  let mut guess = String::new();
  io::stdin()
    .read_line(&mut guess)
    .expect("Failed to read line");

  println!("You guessed: {guess}");
}
```

Some explanation...
`let mut guess = String::new();` creates new **mutable** variable, meaning it's value can be changed  
By default variables in Rust are immutable, meaning once it has value assigned, it cannot be changed (`let apples = 5;`)

```rs
let apples = 5;  // immutable
let mut bananas = 6;  // mutable
```

`io::stdin()` is function from Rust's standard library, which we included into our file with `use std::io` at the top  
We are calling `read_line()` function, which will get user input. We are passing parameter `&mut guess`  
The `&` symbol makes it being passed as a **reference**. References are also immutable by default, so we had to do `&mut`  
We are handling potential failure with `expect()` - when something goes wrong, it prints the string  
Under the hood, the `read_line()` takes whatever user types, and stores it into `guess` variable, but also `Result`, which is an _enum_ (more on this later)  
This enum has two possible values (variants) - `Ok` and `Err`. If it's `Ok` then `expect` just returns the value, if it's `Err`, it crashes the app and prints the message  
If we didn't put the `expect()` here, we will get warning saying that it may be `Err` and should be handled

Lastly it prints the value with _placeholder_ (`{}`)  
There are two ways of using this...

```rs
let x = 10;
println!("Value of x is {x}");
println!("Value of x is {}", x);
```

As the next part of the game, we want to generate random number  
Rust does not include this in standard library, but you can use `rand` crate (`cargo add rand`)  
We will have to include (`use`) this crate in our code  
There is updated code with generated random number from range 1 to 10

```rs
use std::io;
use rand::Rng;

fn main() {
    println!("Guessing game!");

    let secret_number = rand::rng().random_range(1..=10);
    // println!("Secret: {secret_number}");

    println!("Enter your guess: ");

    let mut guess = String::new();
    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```

> Note: There is also separate [Rust `rand` book](https://rust-random.github.io/book/) if you want to know more

Now we have user input, and random number, we only have to compare them  
We will use `std::cmp::Ordering` to compare the two variables  
The `cmp` is a method on a number, which takes another number as argument and return `Ordering`  
`Ordering` is an enum, which has 3 variants - `Less`, `Greater` and `Equal`, which I think are self-explanatory  
We can _"do something for every possible value"_ by using `match`  
But we have a problem, the `guess` variable is a `String`, and we need and integer-like variable  
We can simply convert `String` to `u32` (32-bit unsigned integer) with `parse()` method as seen in code

```rs
use std::io;
use std::cmp::Ordering;

use rand::Rng;

fn main() {
    println!("Guessing game!");

    let secret_number = rand::rng().random_range(1..=10);
    // println!("Secret: {secret_number}");

    println!("Enter your guess: ");

    let mut guess = String::new();
    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    // convert to integer
    let guess: u32 = guess.trim().parse().expect("Please enter a number!");

    // evaluating the result
    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You won!")
    }
}
```

With the code above, we specified the type of `guess` variable to `u32`  
Thanks to this, the `parse()` method automatically deduces which type to convert the original variable to  
We could have done this before, for example `let guess: String = String::new();`, but Rust automatically assigned the type thanks to the return type of `String::new()` method

Now the game kinda works, but it would be nice to have multiple guesses  
We can do so with `loop {  }`, which is basically infinite while loop  
We also have to exit this loop with `break` when user guesses right, otherwise it will just run forever

```rs
use std::cmp::Ordering;
use std::io;

use rand::Rng;

fn main() {
    println!("Guessing game!");

    let secret_number = rand::rng().random_range(1..=10);

    loop {
        println!("\nEnter your guess: ");

        let mut guess = String::new();
        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        // convert to integer
        let guess: u32 = guess.trim().parse().expect("Please enter a number!");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You won!");
                break;
            }
        }
    }
}
```

Now another potential problem is the conversion, because when user enters non-numeric value, the program crashes  
We can handle this with `match`, since it's enum (we already know that the possible values for `parse()` are `Ok` and `Err`)

```rs
use std::cmp::Ordering;
use std::io;

use rand::Rng;

fn main() {
    println!("Guessing game!");

    let secret_number = rand::rng().random_range(1..=10);

    loop {
        println!("\nEnter your guess: ");

        let mut guess = String::new();
        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        // convert to integer
        // let guess: u32 = guess.trim().parse().expect("Please enter a number!");
        let guess: u32 = match guess.trim().parse() {
            Ok(value) => value,
            Err(_) => {
                println!("Please enter a number!");
                continue;
            }
        };

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You won!");
                break;
            }
        }
    }
}
```

## Common programming concepts

As mentioned before, variables are immutable by default - once a value is bound to a name, you can't change the value  
For example, the code bellow does not compile because of this

```rs
fn main() {
  let x = 5;
  println!("Value of x: {x}");
  x = 6;
  println!("Value of x: {x}");
}
```

The compiler is your friend and tells you some important info  
You can see that _"cannot assign twice to immutable variable"_ and _"consider making this binding mutable"_  
You can see another example with `rustc --explain E0384` as said bellow

```txt
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         - first assignment to `x`
3 |     println!("Value of x: {x}");
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable
  |
help: consider making this binding mutable
  |
2 |     let mut x = 5;
  |         +++

For more information about this error, try `rustc --explain E0384`.
```

### Constants

```rs
const X = 10;
const PI = 3.14;
const A: u32 = 20;
```

While _variables_ are immutable by default, _constants_ are always immutable, and cannot be mutable  
You are not allowed to use `mut` with constants

With variables, you can declare it and assign its value later  
With constants, you have to declare **and** assign its value

Convention says that constants should be named with `UPPER_SNAKE_CASE`

You can have **global** constants, but not global variables

```rs
const X = 10;
// let y = 10;  // this will not compile

fn main() {
  println!("Value of X = {X}");
}
```

Constants are valid for entire program run  
Constants are valid within the scope they were declared in  
Constants may be set only to a constant expression, not the result of a value that could only be computed at runtime

### Shadowing

Shadowing is done when declaring two variables with the same name  
In the example bellow, the first variable is shadowed by the second variable

> Note: you will get a warning about unused variable, but we will ignore it just now

```rs
let x = 10;
let x = 20;
```

In other languages like C, Java or Python you cannot declare the same variable more than once, but Rust is just OK with this

You can also play with scope using `{ }`, you can do something like the code bellow  
Variables are only valid in within the scope they were declared in, and we can create scopes within scoped, so this is perfectly valid

```rs
fn main() {
    let x = 10;
    println!("First: {x}");  // prints 10

    {
        let x = x * 2;
        println!("Inner scope: {x}");  // prints 20
    }

    println!("Second: {x}");  // prints 10

    let x = x + 1;
    println!("Third: {x}");  // prints 11
}
```

When using shadowing, we can also change the type of the variable, which is not possible if we had mutable variable

```rs
let spaces = "    ";  // string
let spaces = spaces.len();  // integer

let mut dots = "...";
dots = dots.len();  // error here
```

### Data Types

Every variable and constant is of a certain data type  
In rust there are two main categories of data types - **scalar** and **compound**

Rust is _statically typed language_, meaning that it has to know the types of all variables at compile time  
The compiler usually knows the type of variable based on it's value. If we do not provide the value, we have to specify the type manually

```rs
let a = 10;  // ok
let b;  // not ok
let c: u32;  // ok
```

#### Scalar Types

Scalar type represents a single value  
Rust has 4 scalar types - **integers**, **floating-point numbers**, **booleans** and **characters**

**Integer Types**

| Length                   | Signed  | Unsigned |
| ------------------------ | ------- | -------- |
| 8-bit                    | `i8`    | `u8`     |
| 16-bit                   | `i16`   | `u16`    |
| 32-bit                   | `i32`   | `u32`    |
| 64-bit                   | `i64`   | `u64`    |
| 128-bit                  | `i128`  | `u128`   |
| _Architecture Dependent_ | `isize` | `usize`  |

Architecture Dependent types depends on your architecture :)  
If you have 32-bit architecture then it's 32 bits, if you have 64-bit architecture then it's 64 bits

Rust's default are `i32` for general integers, and `usize` for indexing some form of collection

**Number Literals**

| Number Literal   | Example       |
| ---------------- | ------------- |
| Decimal          | `12_345`      |
| Hex              | `0xff`        |
| Octal            | `0o77`        |
| Binary           | `0b1100_1100` |
| Byte (`u8` only) | `b'A'`        |

**Floating-point Numbers**

| Size   | Example |
| ------ | ------- |
| 32-bit | `f32`   |
| 64-bit | `f64`   |

The default for float is `f64` in Rust

**Boolean**

```rs
let t: bool = true;
let f: bool = false;
```

**Character**

Specified with single quotes (`''`)  
4 Bytes  
Represents a _Unicode scalar value_

```rs
let x = 'a';
let y: char = 'A';
let z = '🌳';
```

#### Compound Types

Compound Types can group multiple values into one type  
Values can be either Scalar (integers, chars, ...) or another Compound types

Rust has two Compound Types - **Tuples** and **Arrays**

##### Tuples

Used to group together values of different types  
Declared by writing comma-separated values inside parentheses (`(` and `)`)

```rs
let tup = (16, "ahoj", 4.3);
let tup2: (char, i8, f64) = ('a', 5, 3.14);  // with specified types
```

To access values, you can do something like this

```rs
let tup = (500, 600, 700);

// accessing value on specific index
let a = tup.0;
let b = tup.1;
let c = tup.1;

// decomposition
let (a, b, c) = tup;
```

##### Arrays

Similar to Tuples, Arrays group together multiple values  
The difference is that in Array, all values have to be of the same type  
Declared by typing comma-separated values into square brackets (`[` and `]`)

```rs
let numbers = [5, 3, 16, 2, 7];
```

If you want to specify type, you have to also specify the number of elements  
These are written inside square brackets, and separated by semicolon

```rs
let numbers: [i32; 4] = [1, 2, 3, 4];
```

If you want, you can fill it with the same values at the start  
In the example below, the array will have 10 elements, where each element will be `x`;

```rs
let arr: [char; 10] = ['x'; 10];
```

You can access values with index

```rs
let numbers = [5, 3, 16, 2, 7];

let a = numbers[0];
let b = numbers[1];
let c = numbers[2];
```

### Functions

Rust uses `snake_case` naming convention for functions

```rs
fn main() {
  println!("Hello from main");
  another_function();
}

fn another_function() {
  println!("Hello from another function");
}
```

**Parameters**  
You have to declare the type

```rs
fn main() {
    print_age(99);
}

fn print_age(n: u32) {
    println!("Your age is {n}");
}
```

**Returning values**

```rs
fn main() {
    let x = incrment(5);
    println!("{x}");
}

fn incrment(n: i32) -> i32 {
    return n + 1;  // adds 1 to 'n'
}
```

There is _"more Rust"_ approach to returning values  
You don't have to write the `return` and `;`

```rs
fn increment(n: i32) -> i32 {
  n + 1
}

fn five() -> i32 {
  5
}
```

#### Statements vs. Expressions

**Statements** are instructions that perform some action and **do not return a value**  
**Expressions** evaluate to resultant value

```rs
let x = 5;  // statement
let y = 2 + 2;  // statement

some_function_that_returns_something();  // expression

let y = {
  let x = 1;  // statement
  x + 1  // expression, this is being returned
}
```

### Control flow

#### `if` statements

```rs
fn main() {
  let age = 10;

  if age >= 18 {
      println!("You are adult");
  } else if age == 17 {
      println!("You are almost adult");
  } else {
      println!("You are kid");
  }
}
```

I also these _"inline ifs"_  
Both values have to be the same type btw

```rs
let is_raining = false;
let clothes = if is_raining { "raincoat" } else { "tshirt" };

let something = if is_raining { 10 } else { "hello" };  // error here - type mismatch
```

#### `loop` - infinite loop

```rs
loop {
  println!("Hello");
}
```

##### Returning value from `loop`

```rs
let mut counter = 1;
let result = loop {
    println!("Hello {counter}");

    if counter == 10 {
        break counter * 2;
    }

    counter += 1;
};

println!("Result is {result}");
```

##### Loop Labels

If you have multiple nested loops, `break` and `continue` apply to the innermost loop  
But if you want to `break`/`continue` to different, you can use _loop labels_ to kind of give a name to a loop

```rs
let mut count = 0;
'counting_up: loop {
  println!("count = {count}");
  let mut remaining = 3;

  'inner: loop {
    if remaining = 0 {
      break;
    }

    if count = 10 {
      break 'counting_up;  // breaking 'counting_up' instead of 'inner'
    }
    remaining -= 1;
  }
  count -= 1;
}
```

#### `while` loop

```rs
let mut count = 0;

while count < 5 {
  println!("Still looping: {count}");
  count += 1;
}
```

#### `for` loop

Looping over array

```rs
let numbers = [1, 4, 0, 7, 23, 6];

for i in number {
  println!("Current value: {i}");
}
```

Looping over a `Range`  
The second number in range is exclusive

```rs
for i in 0..10 {
  println!("Current value: {i}");
}

for i in (0..11).rev() {
  println!("Countdown: {i}");
}
```

## Ownership

Ownership is a set of rules that govern how a Rust program manages memory  
If any of the rules are violated, the program won't compile

**Ownership rules**

- Each value in Rust has an _owner_
- There can only be one owner at a time
- When the owner goes out of scope, the value will be dropped

### Memory and allocation

Memory is automatically freed when it's owner goes out of scope  
Usually the `}` symbol "ends" the scope

```rs
{
  let s = String::from("hello");
  println!("{s}");  // ok
}
println!("{s}");  // not ok
```

You can manually free memory by calling the `drop` function

There is a difference between `String` type (`let s = String::from("ahoj")`) and _String literal_ (`let s = "ahoj"`), which is of type `&'static str`  
String literal is immutable (like you can change it whole, cannot change part of it), String can be mutable

```rs
    let x = 5;
    let y = x;
    println!("{x}");  // ok
    println!("{y}");

    let x = "s";
    let y = x;
    println!("{x}");  // ok
    println!("{y}");

    let x = String::from("s");
    let y = x;
    println!("{x}");  // error here bcs String is allocated on heap, should use `x.clone()` earlier
    println!("{y}");
```

In this case the memory is moved, not copied, so we get an error because the first variable is no longer valid  
String does not implement the `Copy` trait
Usually (but not always) types allocated on stack does implement the `Copy` trait  
These are usually types of known size (at compile time) - `i32`, `char`, `bool`, `f64`, tuples of these values only - but not Strings

Rust won't let you annotate the `Copy` trait to type, if the type of any of its parts has implemented the `Drop` trait

If we want to make a **deep copy**, we have to use the `clone` method  
In example bellow, you can see that when we change `y`, `x` remains unchanged

```rs
let x = String::from("ahoj");
let mut y = x.clone();
y += " kamarat";

println!("{x}\n{y}");
```

The same goes for values passed to a function - if we pass `i32` for example - it gets copied; if we pass `String` - it gets transferred

```rs
fn main() {
    let s = String::from("ahoj");
    steals_ownership(s);

    println!("{s}");  // error
}

fn steals_ownership(val: String) {
    println!("{val}");
}
```

The same goes with returning values...

```rs
fn main() {
    let x = gives();
    let y = String::from("hey");
    let z = takes_and_gives(y);
}

fn gives() -> String {
    String::from("hello")
}

fn takes_and_gives(val: String) -> String {
    println!("{val}");
    val
}
```

In Rust, you can return multiple values with tuples

```rs
fn something() -> (String, i32) {
  (String::from("ahoj"), 9)
}
```

It feels a bit complicated to pass and give back the value all the time  
Also, if the function was calculating and returning something, we will have to return 2 values  
But this is too much work...  
That's why Rust does have something called _references_

### References and Borrowing

With reference, you can pass a value to a function without transferring the ownership  
You can specify reference by adding `&` symbol before the type

```rs
fn main() {
  let s = String::from("ahoj");

  let lenght = calculate_length(&s1);
  let (lenght, s) = calculate_length_without_reference(s);
}

fn calculate_length(s: &String) -> usize {
  s.len()
}

fn calculate_length_without_reference(s: String) -> (usize, String) {
  (s.len(), s)
}
```

There is unwritten rule that says that _functions do not take ownership of their arguments unless they need to_  
Reasons for that will become clear as we keep going

Be aware that references (same as variables) are immutable by default  
So if you want to modify a reference inside function, you have to make a mutable reference  
Also the original variable has to be mutable

```rs
fn main() {
    let mut s = String::from("hello");
    add_something(&mut s);
    println!("{s}");
}

fn add_something(s: &mut String) {
    s.push_str(" world");
}
```

**Rules of references**

- You can either have **one mutable reference** or **multiple immutable references**
- References must always be valid

So if you already have one mutable reference, you can't have any more references (neither mutable, nor immutable)  
If you already have immutable reference, you can only have other immutable references, not any more mutable  
This mechanism prevents some undefined behavior in Rust

```rs
let mut s = String::from("ahoj");

let r1 = &mut s;
let r2 = &mut s;  // error
```

The code bellow is perfectly valid, because you only have one reference at a time

```rs
let mut s = String::from("ahoj");

{
  let r1 = &mut s;
}  // r1 goes out of scope here

let r2 = &mut s;
```

Another funny thing is that **reference scope** starts where it's introduces, and ends where _it's last used_  
This rule makes the code bellow perfectly fine

```rs
let mut s = String::from("hello");

let r1 = &s;
let r2 = &s;

// let r3 = &mut s;  // this will cause error here

println!("Using r1 and r2 here: {r1} {r2}");
// after this, r1 and r2 will not be used

let r3 = &mut s;
println!("Using r2 here: {r3}");
```

#### Dangling references

Similar to dangling pointers in languages like C/C++, where you can have pointer to memory that no longer belongs to us  
Rust prevents you to have dangling references - you cannot have reference to something that went out of scope  
So the code bellow will give us error about _lifetime_ (will be discussed later)  
Now it's only important to know that Rust won't let you have dangling reference

```rs
fn main()
    let x = dangling_reference();
}

fn dangling_reference() -> &String {
    let s = String::from("ahoj");
    &s
}  // s is being freed here, returned value points to nothing
```

### Slices

Slices let you reference a part of collection  
It's a kind of reference, so it does not have ownership

#### String slices

Rather than referencing to the entire `String` (for example), slices lets you reference to its portion

```rs
let s = String::from("hello world");
let hello = &s[0..5];
let world = &s[6.11];

let first_two_letters = &s[..2];
// let others = &s[2..s.len()];
let others = &s[2..];

let whole_string = &s[..];
```

Slices of string has their own special type = `&str`

```rs
let name: String = String::from("Peter");
let letters: &str = &name[..2];
```

Slices also prevents you from having invalid/dangling slice  
In the code bellow, on line `let word = ...`, immutable borrow happens, so you cannot borrow it mutably in `clear()`  
If we didn't have the `println` at the bottom, it should work fine, but why would we do this if we didn't need it :)

```rs
fn main() {
  let mut s = String::from("hello world");

  let word = extract_first_word(&s);

  s.clear()  // error here

  println!("{word}");
}

fn extract_first_word(s: &String) -> &str { ... }
```

We can change the signature of `extract_first_word` to take string slice as argument, so it will be more general  
Now we can pass both String, string slice, and either String literal (literals actually are slices already)

```rs
fn main() {
  let mut s = String::from("hello world");

  let a = extract_first_word(&s[..]);
  let a = extract_first_word(&s);

  let mut lit = "hello world";
  let a = extract_first_word(lit[..]);
  let a = extract_first_word(lit);
}

fn extract_first_word(s: &str) -> &str { ... }
```

#### Other slices

You can also use slices on other collection types, not just strings

```rs
let a = [1, 2, 3, 4, 5];
let slice = &a[..3];
```

## Structs

`struct` is a custom data type that lets you package together and name multiple related values, that make up a meaningful group  
Structs are similar to [tuples](#tuples), they can hold multiple values of different types  
Unlike tuples, structs have name for each value - this way you don't have to rely on order, and it's clear what those values mean

```rs
fn main() {
    let jozko = User {
        active: true,
        username: String::from("Jozko"),
        email: String::from("jozko@example.com"),
        age: 20,
    };
    // let jozko = User {true, String::from("Jozko"), String::from("jozko@example.com"), 20};  // this does not work

    println!("{}", jozko.username);
}

struct User {
    active: bool,
    username: String,
    email: String,
    age: u8,
}
```

If we want to make something mutable, the whole instance must be mutable, Rust does not allow only part of it to be mutable

```rs
let mut jozko = User { ... };
jozko.email = "jozik@example.com";
```

There is a quick shorthand

```rs
fn build_user(email: String, username: String) -> User {
  User {
    active: true,
    username,
    email,
    age: 18
  }
}
```

If we wanted to create new user, where most of the field will be the same as the first user, we can do something like this...  
But be careful, this moves the data, as stated in previous chapters  
Types that implement `Copy` are good, others should be used with `clone`

```rs
let user1 = User { ... };

// long way
let user2 = User {
  active: user1.active,
  username: user1.username,
  email: String::from("jozik@example.com"),
  age: 18
};

// short way
let user2 = User {
  email: String::from("jozik@example.com"),
  ..user1
};
```

Example of use

```rs
struct Rectangle {
  width: u32,
  height: u32
}

fn main() {
  let r1 = Rectangle {width: 150, height = 200};

  let a = area(&r1);

}

fn area(r: &Rectangle) -> u32 {
  r.width * r.height
}
```

### Tuple Structs

Is defined as a struct, but does not have named fields (as tuple)

```rs
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
  let black = Color(0, 0, 0);
  let origin = Point(0, 0, 0);
}
```

Note that these are two different types, although they have the same fields of the same type  
If you had a function which takes `Point`, and you will give it `Color`, it will not compile  
This is the difference between tuples - if you had tuple with the same types, it will take it

### Unit-like structs

Tuple that does not have any fields  
I don't understand the use of this, but it's there

```rs
struct AlwaysEqual;

fn main() {
  let subject = AlwaysEqual;
}
```
