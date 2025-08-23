# Rust  

I'm trying to learn Rust :)  

Following [this tutorial](https://www.youtube.com/watch?v=rQ_J9WH6CGk), later added stuff from multiple sources   

## Set up  

### Install

This command should be all you need to compile/run Rust  
You will install Rust with *rustup*   

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

On Windows you might also need to install *Visual Studio C++ Build tools*  

See all info [here](https://www.rust-lang.org/tools/install)  


Confirm with these commands  

```bash
rustup --version
rustc --version
cargo --version
```

--- 

What I did was this...

```bash
yay -S rustup
```

...and it worked 

## Hello world

Create new file like `hello.rs`  

```rust
fn main() {
  println!("Hello, world!");
}
```

Now you need to compile it using `rustc` compiler and run it...  

```bash
rustc hello.rs

./hello
```

## Cargo  

Cargo is package manager for Rust  
Similar to Python's `pip` or C#'s `nuget`  

You should already have Cargo installed, but you can verify with this command  

```bash
cargo --version
```

### New project 

```bash
cargo new my_first_project
```

> Note: Naming should follow `snake_case` or `kebab-case` conventions 

This should create *Hello World* project for you (in new folder)  
If you have a folder already created, you can use `cargo init` instead  

```bash
cd my_first_project
cargo init
```

### Running project 

To compile and run Cargo project, hit this command  

```bash
cargo run
```

## Data types 

### Primitive data types

| Rust type                 | C/C++ type                                    | Description                                       |
| ------------------------- | --------------------------------------------- | ------------------------------------------------- |
| `i8`, `i16`, `i32`, `i64` | `int8_t`, `int16_t`, `int32_t`, `in64_t`      | `8`, `16`, `32` and `64` bit integer              |
| `u8`, `u16`, `u32`, `u64` | `uint8_t`, `uint16_t`, `uint32_t`, `uint64_t` | `8`, `16`, `32` and `64` bit **unsigned** integer |
| `i128`                    |                                               | `64` bit integer                                  |
| `u128`                    |                                               | `64` bit **unsigned** integer                     |
| `f32`                     | `float`                                       | Decimal number with 32-bit precision              |
| `f64`                     | `double`                                      | Decimal number with 64-bit precision              |
| `bool`                    | `bool`                                        | Boolean - `true` or `false`                       |
| `char`                    | `char`                                        | One character                                     |
| `usize`                   | `size_t`                                      | Numbers as large as the address space             |

```rust
fn main() {
	let x: i32 = -43;
	let y: u64 = 1000;
	
	println!("Signed integer: {}", x);
	println!("Unsigned integer: {}", y);
	
	let pi: f32 = 3.14;
	
	println!("Value of PI: {}", pi);
	
	let is_sunny: bool = true;
	
	println!("Is it sunny outside? : {}", is_sunny);
	
	let first_letter: char = 'P';
	
	println!("First letter: {}", first_letter);
}
```

You actually don't have to specify the types, it will figure out automatically, but it's recommended  

You can also put the variable directly into `{}` 

```rust
fn main() {
	let x = 100;
	println!("The value of x is {x}");
}
```

### Compound data types

Categorized into 4 main groups:
- [Arrays](#Arrays)
- [Tuples](#Tuples)
- [Slices](#Slices)
- [Strings](#Strings) (and string slices)

### Arrays 

Array of fixed amount of variables of the same type  

You define the type like `[primitive_data_type, count]`  
In the example bellow, you have 5 variables of `i32` type  

```rust
let numbers: [i32; 5] = [1,2,3,4,5];

// you have to put {:?} instead of just {}  
// something with formatters?
// it should be explained later
println!("My numbers: {:?}", numbers);
```

In the example bellow, you can see *string slice* (explained bellow)   

```rust
let fruits: [&str; 3] = ["apple", "banana", "orange"];
println!("Fruits: {}", fruits[0]);  // first element
println!("Fruits: {}", fruits[1]);  // second element
println!("Fruits: {}", fruits[2]);  // third element
```

### Tuples

Similar to Arrays, but does not have to contain elements of the same type  
Uses `()` instead of `[]`  

```rust
let human: (String, i32, bool) = ("Peter".to_string(), 22, true);
println!("Human Tuple: {:?}", human); 
```

Notice how we have to use the `.to_string()` method to turn `String` into `&str`  
Still have no idea what that is, will be discussed later  

You can have other compound types inside of tuples  

```rust
let mix_tuple = ("Ferko", 23, [1,2,3,4]);
println!("Mix Tuple {:?}", mix_tuple);
```

### Slices

A slice is a *view* into a part of collection  

```rust
let number_slice: &[i32] = &[1,2,3,4,5];
```

It looks like a reference/alias to me, still have no idea what that does, it should be discussed later  

Some more examples  

```rust
let animal_slice: &[&str] = &["Lion", "Elephant", "Crocodile"];
let book_slice: &[&String] = &[&"IT".to_string(), &"Harry Potter".to_string()]; 
println!("Animals: {:?}, Books: {:?}", animal_slice, book_slice);
```

### Strings

Strings and String Slices  

The main difference is that *Strings* are expandable and mutable - you can grow them and change content   
*String Slices* are not mutable, something like `const` in C++?   

*String* is allocated dynamically on the Heap - slow  

*String* is also *Owned type*, not *borrowed* (explained later)  

> Note: By default, all variables are **immutable** by default in Rust
> Meaning that you can assign a value to it just once  
> You can go around this by using the `mut` keyword 

```rust
let mut stone_cold: String = String::from("Hell"); 
println!("Stone cold says: {}", stone_cold);
stone_cold.push_str(" yeah!");
```

**String slice** is a reference, allocated on the stack  
Anything on stack cannot be mutable (I think)  

```rust
let my_string: String = String::from("Hello there");  
let my_string_slice: &str = &my_string;  
let my_string_slice2: &str = &my_string[0..5];
```

## Functions 

Names should follow the `snake_case` naming convention  
The order of function does not matter (unlike C++) - this is called **Hoisting**  

```rust
fn main() {
	hello_world();
}

fn hello_world() {
	println!("Hello world");
}
```

### Parameters  

```rust
fn main() {
    human_stats("Peter", 22, 170.0);  
    // you have to put the decimal part in float
    // otherwise it's gonna give an error
}  
  
fn human_stats(name: &str, age: u32, height: f32) {  
    println!("My name is {}, I am {} years old, I am {} cm tall", name, age, height);  
}
```

### Returning a value

Difference between Expressions and Statements
- Expression - anything that returns a value 
- Statement - anything that does not return a value (often ends with `;`) 

Everything that is at the last line (without `;`) will be returned

```rust
fn main() {
	let a: i32 = add(1, 2);
}

fn add(a: i32, b: i32) -> i32 {  
	// you can do "classic" return statement
    // return a + b;

	// but in rust, we do it like this
    a + b  
}
```

You can also do this, but idk how is this called  

```rust
let area: i32 = {  
    let width: i32 = 20;  
    let height: i32 = 10;  
    width * height  
};  // this will return 200
```

Another **example** - BMI calculation  
BMI (Body Mass Index) is calculated like this: $\dfrac{\text{weight}}{\text{height}^2}$   
> Weight is in kilograms
> Height is in meters

```rust
fn main() {
	let w = 60.0;  
	let h = 1.70;  
	println!("Calculated BMI: {}", calculate_bmi(w, h));  
	println!("Calculated BMI: {:.2}", calculate_bmi(w, h));  // round to 2 decimal places
}

fn calculate_bmi(weight_kg: f32, height_m: f32) -> f32 {  
    weight_kg / (height_m * height_m)  
}
```

## Ownership, Borrowing, References

Rust's way to solve problem with memory allocation, memory leaks, dangling pointers, null pointer dereferencing and garbage collection  

### Ownership 

Ownership rules  
- Every value has an *owner*
- There can be only one owner at a time  
- When the owner goes out of scope, the value will be dropped

Each value in Rust has a variable that's its owner  

In the example bellow, you can see using the variable without changing the owner  

```rust
fn main() {
	let s1 = String::from("rust");  // value 'rust' is only owned by the 's1' owner
	let length = calculate_lenght(&s1);  // passing reference, not value

	println!("Lenght of {} is {}", s1, length);
}

fn calculate_length(s: &String) -> usize {  // parameter is a reference, not value
	s.len()
}
```

In the example bellow, you can see how to change the owner  

```rust
fn main() {
	let s1 = String::from("rust");
	let s2 = s1;  
	// you just transferred the ownership from 's1' to 's2'

	println!("{}", s2);  // this is ok
	// println!("{}", s1);  // this will give error 
}
```

The 3rd rule is pretty obvious, you cannot access values that are out of scope (unless passed by a parameter)  

### Borrowing

Very similar to C++'s references - the `&` symbol  

```rust
let x: i32 = 5;
let y: &i32 = &x;
```

By default, references are *immutable*, such as everything *(?)* in Rust  
You can also make *mutable* reference with `&mut`, if you want to change the value of it  
If you want to make *mutable* reference, the owner must be *mutable* as well  

```rust
let mut x = 5;
let y = &mut x;

*y += 1;

println!("Value of x = {}", x);  // prints 6
```

But if we wanted to print the value of `y`, we will get error saying *Cannot borrow 'x' as immutable, because it is also borrowed as mutable*  
This leads to another rule - **You can have only *one mutable* reference, or *many immutable* references**  

I will use `struct` here  
It's very similar to C/C++   
It's a data structure that allows you to group multiple fields together under one name  
The difference is, you can implement methods with `impl` keyword  

```rust
fn main() {
    let mut account = BankAccount{owner: "Peter".to_string(), balance: 1000.0};
    // immutable borrow
    account.check_balance();
    // mutable borrow
    account.withdraw(200.0);
    // immutable borrow
    account.check_balance();
}

struct BankAccount {  
    owner: String,  
    balance: f64  
}  
  
impl BankAccount {  
    fn withdraw(&mut self, amount: f64) {  
        println!("Withdrawing {} from account owned by {}", amount, self.owner);  
        self.balance -= amount;  
    }  
      
    fn check_balance(&self) {  
        println!("Account owned by {} has the balance of {}", self.owner, self.balance);  
    }  
}
```

## Variables, Constants and Mutability  

### Mutability

Variables in Rust are immutable by default - meaning you cannot change them  
Once it's declared, initialized, assigned, whatever, you can't change it's value  
This only goes with variables, not *constants*  
So if you tried to change the value of assigned variable, you will get error  

```rust
let x = 10;
x = 11;  // error here
// cannot assign twice to immutable variable 'x'
```

To fix this, you *"should consider"* making it mutable (the compiler said so)  

```rust
let mut x = 10;
x = 11;
```

You don't have to put the type of the variable, compiler finds out automatically  
But, if you decide not to put it here, you have to prefix the variable name with `_`  

```rust
let x: u32 = 5;  // we said that it is 'u32'
let _y = 10;  // compiler decides it is 'u32' 
```

### Constants

You declare constant by `const` keyword, instead of `let`  
There are few **rules**: 
- Constants are immutable, and can never be mutable  
- You have to specify the type of constant
- You should give an uppercase name to constant

```rust
let x = 10;  // ok
let mut y = 11;  // ok

const Z: i32 = 12;  // ok
const Z = 12;  // not ok - type not specified
const z: i32 = 12;  // warning - name should be uppercase
const mut z = 12;  // not ok - mutable
```

You can define **global constants**, but you can't define global variables (it suggests adding static)  

```rust 
fn main() {
	println!("Value of PI is {}", PI)
}

const PI: f32 = 3.14;
```

## Shadowing

```rust
fn main() {
	let x = 5;
	let x = x + 1;  // the 'let' is important
	// first 'x' was 'shadowed' by the second 

	{
		let x = x*2;
		println!("Inner scope: {}", x);  // prints '12'
	}
	
	println!("Outer scope: {}", x);  // prints '6'
}
```

Shadowing is not the same thing as marking a variable as mutable  

Variables don't even have to be the same type  

```rust
let spaces: &str = "    ";
let spaces: usize = spaces.len();
```

If you tried to do it without shadowing (using mutable variable) you will get error

```rust
let mut spaces = "    ";  
spaces = spaces.len();  // error here - expected '&str', found 'usize'
```

## Comments 

```rust
fn main() {
	// this is one line comment
	// printing hello world
	println!("Hello world");

	/* 
	this
	is
	multiline
	comment 
	*/
}
```

These are *non-doc* comments  
There are also *doc* comments and attributes, but I don't really understand what's going on, you can check it at [this link](https://doc.rust-lang.org/rust-by-example/meta/doc.html)

## Control flow 

### `if`, `else if`, `else`

```rust
let age = 22;

if age > 18 {  
    println!("You are adult");  
} else if age == 18 {
	println!("You are barely adult");
} else {  
    println!("You are not adult");  
}
```

You can also put `()` here, but it's unnecessary/redundant  

You can also do inline `if`  
Be aware that both values must be the same type  

```rust
let condition = true;
let number = if condition {5} else {6};
let number = if condition {5} else {"six"};  // error here
```

### Looping mechanisms

#### `loop`

Unconditional loop  
Keeps running, until you stop it with `break`  
Sounds like *while true* for me  

```rust
loop {
	println!("Hello");
}
```

According to documentation, `loop` is used to retry an operation you know might fail  
You can assign a result of loop to a variable  

```rust
let mut counter = 0;  
  
let result = loop {  
    counter += 1;  
      
    if counter == 10 {  
        break counter * 2  // the value will be returned
    }  
};  
  
println!("Result is {result}");
```

To deal with **nested `loop`** , you can add **label** to `loop`  
Because by default, `break` is linked with the inner most loop, but you can control the outer loop with Rust as well  

```rust
let mut count = 0;
'counting_up: loop {  // outer loop
	println!("count = {count}");
	let mut remaining = 10; 
	
	loop {  // inner loop
		println!("remaining = {remaining}");
		if remaining == 9 {
			break;
		}
		
		if count == 2 {
			break 'counting_up;
		}
		
		remaining -= 1;
	}
}
```

#### `while`

```rust
let mut count = 0;

while count < 10 {
	println!("Current count is: {count}");
	count += 1;
}
```

#### `for`

```rust
let a = [1, 2, 3, 4, 5];

for element in a {
	println!("{element}");
}
```

## Warnings

You might see some warnings from compiler  
Do ignore them, you can add this line of code at the very top of the file  

```rust
#![allow(warnings)]

fn main() {

}
```

## Struct

Structs are similar to tuples - holds multiple values of multiple types   
Each field in tuple is named (not numbered like in tuples?)  
Structs in C++ can have methods/functions/procedures (unlike in C++)  

```rust
fn main() {
	struct User {  
	    username: String,  
	    email: String,  
	    active: bool,  
	    sign_in_count: u64  
	}  
	  
	let user1 = User {  
	    username: "jozkoferko".to_string(),  
	    email: "jozko@ferko.com".to_string(),  
	    active: true,  
	    sign_in_count: 0  
	};

	println!("{}", user1.email);
	// println!("{user1.email}");  // this doesnt work for some reason
}
```

Entire struct instance must be mutable to modify it  

```rust
let mut user1 = User{...};  

user1.email = "ferko@jozko.com";
```

You can return struct from a function  
If you have variable, which matches the struct's field name, you can use it simply like this

```rust
fn build_user(username: String, email: String) -> User {
    User { 
		username,  // variable names are the same
		email,  // variable names are the same
		active: true, 
		sign_in_count: 0
    }
}
```

You can reuse older instances of the same struct  
Let's say that you want to only change the email, and leave other values the same as `user1`  

```rust
let user2 = User {
	email: "ahoj@ahoj.sk",
	..user1
};
```

### Tuple structs

Something between tuples and structs?  
Not really sure whats going on  

```rust
struct Color(i32, i32, i32);

let black = Color(0, 0, 0);
let red = Color(255, 0, 0);
```

### Unit-Like struct

Now I really have no idea what is going on  
This "struct" has no fields  

```rust
struct AlwaysEqual;
let subject = AlwaysEqual;
```

### Structs from rustlings

Rust have 3 types of structs 
- Regular Struct
- Tuple Struct
- Unit Struct

Defining

```rust
struct ColorRegularStruct {
	red: u8,
	green: u8,
	blue: u8
}

struct ColorTupleStruct (u8, u8, u8);

struct UnitStruct;  // i still dont know the use of this
```

Instantiating and accessing values

```rust
let c = ColorRegularStruct {
	red: 0,
	green: 255, 
	blue: 0
}
println!("{c.red} {c.green} {c.blue}");

let c = ColorTupleStruct { 0, 255, 0 }
println!("{c.0} {c.1} {c.2}");

let c = UnitStruct;
println!("{c}");
```

## `enum`

Data structure, which represents something, that can be only one of several possible variants  

```rust
fn main() {
	// using variables
	let four = IpVersion::V4;
	let six = IpVersion::V6;
		
	// using functions
	route(IpVersion::V4);
	route(IpVersion::V6);
}

enum IpVersion {
	V4, 
	V6
}

fn route(ip_kind: IpVersion) {
}
```

You can use it in Structs 

```rust
struct IpAddr {
	kind: IpVersion,
	address: String
}

let home = IpAddr {
	kind: IpVersion::V4,
	address: "127.0.0.1".to_string():
}

let loopback = IpAddr {
	kind: IpVersion::V6,
	address: "::1".to_string():
}
```

### Storing data in `enum`

This is going to have similar effect than the previous example with Struct, but purely with Enum

```rust
enum IpAddr {
	V4(String),
	V6(String)
}

let home = IpAddr::V4("127.0.0.1".to_string());
let loopback = IpAddr::V6("::1".to_string());
```

### Enhanced `enum`

The value types can be different for different fields  

```rust
enum IpAddr {
	V4(u8, u8, u8, u8),
	V6(u16, u16, u16, u16, u16, u16, u16, u16)
}

let home = IpAddr::V4(127, 0, 0, 1);
let loopback = IpAddr::V6(0, 0, 0, 0, 0, 0, 0, 1);
```

You can also have it like this  

```rust
struct Point{ ... }

enum Message {  
    Resize{height: i32, width: i32},  // tuple here
    Move(Point),  // struct here
    Echo(String),   // type here
    ChangeColor(u8, u8, u8),  // array? here
    Quit   // nothing here
}
```

Another example  

```rust
struct State {  
    width: u64,  
    height: u64,  
    position: Point,  
    message: String,  
    // RGB color composed of red, green and blue.  
    color: (u8, u8, u8),  
    quit: bool,  
}  
  
impl State {  
    fn resize(&mut self, width: u64, height: u64) {  
        self.width = width;  
        self.height = height;  
    }  
  
    fn move_position(&mut self, point: Point) {  
        self.position = point;  
    }  
  
    fn echo(&mut self, s: String) {  
        self.message = s;  
    }  
  
    fn change_color(&mut self, red: u8, green: u8, blue: u8) {  
        self.color = (red, green, blue);  
    }  
  
    fn quit(&mut self) {  
        self.quit = true;  
    }  
  
    fn process(&mut self, message: Message) {  
        // TODO: Create a match expression to process the different message  
        // variants using the methods defined above.  
        match message {  
            Message::Resize { width, height } => self.resize(width, height),  
            Message::Move(point) => self.move_position(point),  
            Message::Echo(s) => self.echo(s),  
            Message::ChangeColor(r, g, b) => self.change_color(r, g, b),  
            Message::Quit => self.quit()  
        }  
    }  
}
```

## Error handling

There are two approaches 
- `Option<T>`
- `Result<T,E>`

These are basically enums, and looks like this  

```rust 
// don't put this in your code 

enum Option<T> {  
    Some(T),  // represents a value  
    None  // represents absence of value  
}  
  
enum Result<T, E> {  
    Ok(T),  // represents a value  
    Err(E)  // represents an error  
}
```

### `Option<T>` 

```rust
fn main() {
	let result = divide(10.0, 2.0);
	
	match result {
		Some(x) => println!("Result is {x}"),
		None => println!("Couldn't divide")
	};
}

fn divide(a: f64, b: f64) -> Option<f64> {  // function returns 'Option<f64>'
    if b == 0.0 {
        None
    } else {
        Some(a / b)
    }
}
```

### `Result<T,E>`

```rust
fn main() {
    let result = divide(10.0, 0.0);
    
    match result {
        Ok(x) => println!("Result: {x}"),
        Err(x) => println!("Error: {x}")
    };
}

// first Result type parameter - when value is correct
// second Result type parameter - when value is incorrect
fn divide(a: f64, b: f64) -> Result<f64, String> {  // function returns 'Result<f64, String>'
    if b == 0.0 {
        Err("Can't divide by zero".to_string())
    } else { 
        Ok(a / b)
    }
}

```

## Collection Types

### Vector - `Vec<T>`  

Vector allows you to store more than one value in a single data structure  
Vectors can only store values of the same type  

Creating empty vector  

```rust
let v: Vec<i32> = Vec::new();  // immutable - cannot add/remove values
// v.push(1);  // error

let mut v: Vec<i32> = Vec::new();  
v.push(1);  // ok
```


Creating vector with values   
Rust has macro for this  

```rust
let v: Vec<i32> = vec![1, 2, 3, 4];
```

Printing from vector  

```rust
println!("{}", v[0]);  // first element
println!("{:?}", v);  // all elements

let first: &i32 = &v[0];  // reference - not taking ownership 
let first: Option<&i32> = v.get(0);  // using the 'get' method - returns 'Option<&i32>'  
```

#### Mapping

```rust
// parameter is array of 'i32' variables
fn vec_map(input: &[i32]) -> Vec[i32] {
	// adding 1 to each element
	input.iter().map(|element| element + 1).collect()  
}

fn vec_map2(input: &[i32]) -> Vec[i32] {
	input
		.iter()
		.map(|element| {
			element * 2
		})
		.collect()
}
```

#### Clone

Had this thing on rustlings  

Problem in the code bellow is that the ownership of the `vec1` variable was transferred, thus it's no longer available in the print statement  

```rust
fill_vec(vec: Vec<i32>) -> Vec<i32> { /* add value to vec */ }  

fn main() {
	let vec1 = vec![1, 2, 3];
	let vec2 = fill_vec(vec1);

	println("{:?}", vec1);  // error - vec1 is not accessible
	println("{:?}", vec2);
}
```

Solution - using `clone()` function

```rust
// ...
let vec2 = fill_vec(vec1.clone());
// ...
```

### UTF-8 Strings

```rust
let s = "hello".to_string();
let s = String::from("hello");
```

If you want to modify it, you have to make it mutable  

```rust
let mut s = "hello".to_string();
println!("{s}");

s.push('a');  // push only one character
println!("{s}");

s.push_str("aaa");  // push multiple characters
println!("{s}");
```

You can add multiple strings together  
But be aware! **The ownership has been transferred**  from `s1` to `s3`, meaning `s1` is no longer available  

```rust
let s1: String = "hello, ".to_string();
let s2: &str = "world";
let s3: String = s1 + s2;  
println!("{s3}");
```

You can also concatenate string with `format`  

```rust
let a = "ahoj".to_string();  
let b = "serus".to_string();  
let c = format!("{a} {b}");  
println!("{c}");
```

Some methods on strings  

```rust
fn trim_me(input: &str) -> &str {  
    // Remove whitespace from both ends of a string.  
    input.trim()  
}  
  
fn compose_me(input: &str) -> String {  
    let mut s: String = input.to_string();  
    s + " world!"  
}  
  
fn replace_me(input: &str) -> String {  
    // Replace "cars" in the string with "balloons".  
    input.replace("cars", "balloons")  
}  
```

### Hash Maps - `HashMap<K, V>`

```rust
let mut scores = HashMap::new();  
scores.insert("Blue".to_string(), 10);  
// scores.insert("Blue".to_string(), 10);  // error - duplicate keys
scores.insert("Yellow".to_string(), 5);

let score = scores.get("Blue");

match score {
	Some(x) => println!("Score is {x}"),
	None => println!("This team is not present")
};
```

You can directly get the value like this  

```rust
let score = scores.get("Yellow").copied().unwrap_or(0);
```

Iterating through Hash Map  

```rust
println!("Printing all values: ");  

for (key, value) in scores {  
    println!("    {key} {value}");  
}
```
