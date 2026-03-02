# TypeScript

[TypeScript](https://www.typescriptlang.org/) is like [JavaScript](./JavaScript.md) with syntax for types  
It's a strongly typed programming language that builds on JavaScript  
It solves some problems with JavaScript

Learning from ["Learn Typescript" course by Codecademy](https://www.codecademy.com/learn/learn-typescript)

## Data types

Just to know what are we working with, these are TypeScript's data types:

Primitive types:

- `number`
- `string`
- `boolean`
- `null` - absent value
- `undefined` - not initialized value
- `symbol` - represents a unique, immutable value, often used as object keys
- `bigint`

Object types:

- `Object`
- `Array`
- `Tuple`
- `Enum`
- `Function`
- `Class`
- `Interface`

Advanced types

- Union types
- Intersection types
- Literal types
- Mapped types

## Variables

Very similar to JavaScript, but you should add a type annotation

```ts
// when value is assigned, it detects automatically
let name = "Peter";

// if you do not assign value, you should add a type
let lastname: string;
lastname = "Peter";
```

If value is not assigned or specified, it will be as `any` type

```ts
// both expressions will do the same
let name: any;
let name;
```

Something like this will fail, you can't change the type

```ts
let name = "Peter";
name = 123;
```

## Functons

You can (and should) annotate the types of parameters

```ts
function greet(name: string) {
  console.log(`Hello, ${name}!`);
}
```

You can (and should) annotate the types of function return type

```ts
function double(val: number): number {
  return val * 2;
}
```

If function does not return anything, that is `void`

```ts
function greet(name: string): void {
  console.log(`Hello ${name}!`);
}
```

You can specify optional parameters, but you have to handle all the cases when using them

```ts
function greet(name?: string) {
  console.log(`Hello ${name || "Jozko"}!`);
}

greet("Ferko");
greet();
```

You can provide default value for a parameter

```ts
function greet(name = "Jozko") {
  console.log(`Hello ${name}`);
}

greet("Ferko");
greet();
```

### Rest parameters

Like when you want variable number of parameters

```ts
function addPower(power, ...numsToAdd) {
  let answer = 0;

  for (i = 0; i < numsToAdd.length; i++) {
    answer += numsToAdd[i] ** p;
  }
}
```

With type annotations - rest parameters are just an array

```ts
function addPower(power: number, ...numsToAdd: number[]): number {
  let answer = 0;

  for (i = 0; i < numsToAdd.length; i++) {
    answer += numsToAdd[i] ** p;
  }
}
```

Just be aware, the bellow code will produce an error

```ts
function range(...nums: number[]) {
  for (i = 0; i < nums.length; i++) {
    console.log(i);
  }
}

range([1, 2, 3, 4]); // error here
range(1, 2, 3, 4); // correct way
```

You can also specify no elements

```ts
function range(...nums: number[]) {
  // do something
}

range(1, 2, 3); // valid
range(); // also valid
```

## Complex types

### Arrays

A collection of multiple variables of the same type  
Type annotations for arrays can look like this

```ts
// both statements will give the same result
let names: string[] = ["Jozko", "Ferko", "Jurko"];
let names: Array<string> = ["Jozko", "Ferko", "Jurko"];
```

Multidimentional arrays

```ts
let hello: string[][] = [
  ["a", "b", "c"],
  ["d", "e", "f"],
];
```

Access elements

```ts
let names: string[] = ["Jozko", "Ferko", "Jurko"];
let x = names[0]; // "Jozko"
```

#### Spread syntax

Like when function has a lot of parameters, and you have them in a tuple, so you don't have to do `func(tup[0], tup[1], tup[2])`, but you do `func(...tup)` instead

```ts
function performDanceMove(
  moveName: string,
  moveReps: number,
  hasFlair: boolean,
): void {
  // do something
}

let danceMoves: [string, number, boolean][] = [
  ["chicken beak", 4, false],
  ["wing flap", 4, false],
  ["clap", 4, false],
];

// simplest example
let m = danceMoves[0];
performDanceMove(...m);

// use all moves with for loop
for (i = 0; i < danceMoves.length; i++) {
  performDanceMove(...danceMoves[i]);
}

// use all moves with foreach loop
danceMoves.forEach((val) => {
  performDanceMove(...val);
});
```

Another example

```ts
function policy(name: string, age: number, minor: boolean) {
  if (minor || age < 18) {
    console.log("No alcohol");
  }
}

let person: [string, number, boolean] = ["Jozko", 21, False];
policy(...person);
```

### Tuples

A collection of multiple variables, each can be different type (don't have to)

```ts
// for example: name, age, is married
let personInfo: [string, number, boolean] = ["Peter", 22, False];
```

It has to have the same amount of values as you provided

```ts
let nums: [number, number] = [1, 2, 3, 4]; // error here
```

Access elements just like in array

```ts
let personInfo: [string, number, boolean] = ["Peter", 22, False];
let x = personInfo[0]; // "Peter""
```

### Enums

```ts
enum Direction {
  North,
  South,
  East,
  West,
}

let myWay: Direction = Direction.North;

let x = Direction.North == 0; // true
```

Enums have numbering, by default starting at `0`

```ts
let x = Direction.North == 0; // true
```

You can start with different number if you want

```ts
enum Direction {
  North = 7, // will be 7
  South, // will be 8
  East, // will be 9
  West, // will be 10
}
```

You can give each item different value if you want

```ts
enum Direction {
  North = 7,
  South = 4,
  East = 2,
  West = 15,
}
```

#### String enums

The thing before was _Numeric enum_, you can also have _String enums_ where the values will be strings instead of numbers

> ~~I didn't quite get what is the point of this from the course~~

When you print some enum value (like `console.log(Direction.North)`), you will get the value of the string you provided (in this case `NORTH`)

```ts
enum Direction {
  North = "NORTH",
  South = "SOUTH",
  East = "EAST",
  West = "WEST",
}
```

### Object types

Feels similar to tuples, but these also have names for the variables  
Notice how objects are defined in curly braces (`{}`) instead of square brackets (`[]`)

```ts
let person: { name: string; age: number } = { name: "Jozko", age: 22 };
```

### Type aliases

If you don't want to type the same object type over and over, you can add type alias

```ts
type Person = { name: string; age: number };

let jozi: Person = { name: "Jozko", age: 22 };
```

You can do this for both `Objects` and `Tuples`

```ts
type Person = [string, number];

let jozi: Person = ["Jozko", 22];
```

### Function types

Functions can be assigned to variables

```ts
let somename = console.log;
somename("hello there");
```

Cool example

```ts
// Math Operations
function add(a, b) {
  return a + b;
}
function multiply(a, b) {
  return a * b;
}

// Add your function type below:

// Math Tutor Function That Accepts a Callback
function mathTutor(operationCallback) {
  console.log("Let's learn how to", operationCallback.name, "!");
  let value25 = operationCallback(2, 5);
  console.log(
    "When we",
    operationCallback.name,
    "2 and 5, we get",
    value25,
    ".",
  );
  console.log(
    "When we",
    operationCallback.name,
    value25,
    "and 7, we get",
    operationCallback(value25, 7),
    ".",
  );
  console.log("Now fill out this worksheet.");
}

// Call your functions below:
mathTutor(add);
mathTutor(multiply);
```

Extended and corrected code  
We are creating a type to check if the passed function has the correct signature

```ts
// Math Operations
function add(a, b) {
  return a + b;
}
function subtract(a, b) {
  return a - b;
}
function multiply(a, b) {
  return a * b;
}
function divide(a, b) {
  return a / b;
}
function wrongAdd(a, b) {
  return a + b + "";
}

// Add your function type below:
type OperatorFunction = (arg0: number, arg1: number) => number;

// Math Tutor Function That Accepts a Callback
function mathTutor(operationCallback: OperatorFunction) {
  console.log("Let's learn how to", operationCallback.name, "!");
  let value25 = operationCallback(2, 5);
  console.log(
    "When we",
    operationCallback.name,
    "2 and 5, we get",
    value25,
    ".",
  );
  console.log(
    "When we",
    operationCallback.name,
    value25,
    "and 7, we get",
    operationCallback(value25, 7),
    ".",
  );
  console.log("Now fill out this worksheet.");
}

// Call your functions below:
mathTutor(wrongAdd);
```

### Generic types

When you have collection of some elements, but you want this collection to have multiple different types of elements, idk how to explain it

```ts
Array<T>;
```

For example

```ts
type Family<T> = {
  parents: [T, T];
  children: T[];
};

let stringFamily: Family<String> = {
  parents: ["Jozko", "Jozka"],
  children: ["acko", "becko", "cecko", "decko"],
};

type Dog = { name: string; tailWaggingSpeed: number };
let dogFamily: Family<Dog> = {
  parents: [
    { name: "Beny", tailWaggingSpeed: 200 },
    { name: "Neviem", tailWaggingSpeed: 10 },
  ],
  children: [
    { name: "Zasran", tailWaggingSpeed: 50 },
    { name: "Fifinka", tailWaggingSpeed: 40 },
    { name: "Jonatan", tailWaggingSpeed: 30 },
  ],
};
```

### Generic functions

```ts
function filledArray<T>(value: T, count: number): T[] {
  let result: T = [];

  for (i = 0; i < count, i++) {
    result.push(value);
  }

  return result;
}

// or with the code provided in the course
function filledArray<T>(value: T, count: number): T[] {
  return Array(count).fill(value);
}

type Dog = [string, number];

// all following arrays will have 6 elements of the appropriate type
let myStrings: string[] = filledArray<string>("hello", 6);
let myNumbers: number[] = filledArray<number>(123, 6);
let myDogs: Dog[] = filledArray<Dog>(["Beny", 1], 6);
```

## Union Types

Unions allows us to specify multiple types for a variable

Example on variable

```ts
let ID: string | number;
ID = 1;
ID = "abc";
```

Example on function parameter

```ts
function justPrintIt(statement: string | number) {
  console.log(`Just printing this: ${statement}`);
}

justPrintIt("hello");
justPrintIt(1);
```

Example if we want different behavior

```ts
function formatValue(value: string | number) {
  if (typeof value === "string") {
    console.log(value.toLowerCase());
  }

  if (typeof value === "number") {
    console.log(value.toFixed(2));
  }
}

formatValue("Hiya");
formatValue(42);
```

Example on return type

```ts
type User = { ... } // some type

function createUser() {
  const randomChance = Math.random() >= 0.5;  // simulate randomness with 50% chance

  if (randomChance) {
    return User { ... };
  } else {
    return "Error while creating user"
  }
}

let something: User | string = createUser();
```

Example with arrays

```ts
function formatSomething(something: (string | number)[]) {
  something.map((val) => {
    if (typeof val === "string") {
      // do something
    } else {
      // do something else
    }
  });
}

const something: (string | number)[] = [
  "123 Main St",
  226800,
  "580 Broadway Apt 4a",
  337900,
];
```

If you are using unions, and all of the types have common attributes, you can use it

```ts
type Dog {
  isPettable: boolean;
  isGoodBoy: boolean;
}

type Goose {
  isPettable: boolean;
  hasFeathers: boolean;
}

let randomAnimal: Dog | Goose = {isPettable = true};

console.log(randomAnimal.isPettable); // ok
console.log(randomAnimal.isGoodBoy); // error
```

Another example

```ts
type Bike {
  pedal: () => void;
  getDirections: () => void;
}

type Subway {
  stand: () => void;
  getDirections: () => void;
}

function travel(mode: Bike | Subway) {
  return mode.getDirections();
}
```

### Unions with literal types

There is not much to say, just see the example

```ts
type Status = "idle" | "downloading" | "complete";

function downloadStatus(status: Status) {
  if ((status = "idle")) {
    console.log("Download");
  }

  if ((status = "downloading")) {
    console.log("Downloading...");
  }

  if ((status = "complete")) {
    console.log("Your download is complete!");
  }
}

downloadStatus("idle");
```
