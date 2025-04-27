# Python

[Python](https://www.python.org/) is a high-level general-purpose programming language  
This is the first language that I learned, so it might look like it :)

Before starting learning this, remember few things

- Programming is hard. It takes years to understand it and to get good in it
- Learn step by step. You don't have to learn all of this in one sitting. Definitely not. Split it into smaller parts
- "Quality over quantity". It's better to learn less and understand it
- "Learn by doing". If you will just read this, you will soon forget all of it. Try the code yourself, experiment, ask, get used to the syntax, learn to work with it

Zen of Python

```
Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

Before getting started, install some text editor like [PyCharm](https://www.jetbrains.com/pycharm/) or [VSCode](https://code.visualstudio.com/)
It's pretty straight forward, but if you struggle, I'm pretty sure you can find some tutorial on how to install it :)  
Now create new file where you will be testing your code  
The file have to have `.py` at the end, for example `main.py`

Now let's get into it :)

# Hello World

Hello World is like the most basic program which is implemented at the start of every tutorial  
It just shows you the way how to get output from the Python script you just wrote

```python
print("Hello World!")
```

```python
print("   /|")
print("  / |")
print(" /  |")
print("/___|")
```

# Variables

Imagine you have a story about character named _John_  
You will write a whole book about _John_  
But later on, you will decide that you don't like the name _John_ and you want to change it to _Mike_  
Now you have to rewrite whole book because the name change

We can use a **variable** to define the name  
We can use the variable instead of the name  
When we will want to change the name, we can change the variable and everything will change

```python
character_name = "John"
character_age = "35"
print("There once was a man named " + character_name + ", ")
print("he was " + character_age + " years old.")
print("He really liked the name" + character_name + ", ")
print("but didn't like being " + character_age + ".")
```

Now, we will re-write the whole story only with changing the value of variable

```python
character_name = "Tom"
character_age = "50"
print("There once was a man named " + character_name + ", ")
print("he was " + character_age + " years old.")
print("He really liked the name" + character_name + ", ")
print("but didn't like being " + character_age + ".")
```

You can also change the variable mid-program, like this

```python
character_name = "John"
character_age = "35"
print("There once was a man named " + character_name + ", ")
print("he was " + character_age + " years old.")

character_name = "Mike"
print("He really liked the name" + character_name + ", ")
print("but didn't like being " + character_age + ".")
```

There are few rules to naming variables:

- Cannot start with numbers
  - `2ndName` is illegal variable name
- Cannot contain whitespaces and dashes
  - `my name` is illegal variable name
  - `my-name` is illegal variable name
  - You can use underscores (`my_beautiful_variable`) or camelCase notation (`myBeautifulVariable`) instead
- Cannot be reserved words
  - Names like `if`, `not`, `while`, `for`, `and`, `or`, `def`, `class` and more, which you will learn about through the course
- Variable names should make sense
  - You should adjust the variable name so it has meaningful name
  - `fl` is not very good, `friends_list` is much better
  - `s` is not very good, `score` is much better
  - If you used just one-letter names you will forget what it means over time, also someone else reading the code might get confused

# Comments

Comment in Python (and other programming languages) is something that you can write, and it will be ignored by program  
It's main purpose is to leave a note for you or another programmer, that is looking at your code  
In Python, you can make a comment with `#`, like this

```python
print("Hello there, how are you")  # this is an comment
```

If Python sees a `#` in a line, it will ignore everything until the end of a line  
Comments does not change the way the code works

# Data Types

There are different types of variables in Python

## String

In example before, we stored `string` in the variable `character_name`, which is basically a text

```python
name = "John"
```

## Number

You can also store a `number` in variable

```python
age = 21
```

> Note: Notice how `string` had quotes (`""`), whereas `number` does not
> In example before we had quotes around a number, just to make some things simpler... You will see [in a moment](#Printing%20string%20and%20number%20together)

## Float

`float` is a number with floating decimal point  
This basically means that it's a number with decimal part

```python
my_float = 3.14
weight = 61.3
```

> Note: Notice how we have decimal dot `.` instead of decimal comma `,`  
> If we wanted to have decimal comma instead, we will got an error  
> Comma in Python is used to things like [lists](#lists) which will be covered later in tutorial

## Boolean

There is also a `boolean` data type, which can be either `True` or `False`

```python
is_john_male = True
is_john_female = False
```

> Note: The first letter or `True` or `False` has to be capital

# Printing string and number together

If you already tried to print a `string` along with a `number` in one print statement, you got an error  
In Python, you can't do this  
There is a way around it, which is to convert `number` to `string`, like this

```python
print(str(my_number))
```

Like this you can convert the number into string, and you can _(obviously)_ use it as string

```python
name = "Peter"
age = 18

print("Hello " + name + " you are " + str(age) + " years old")
```

# Working with `string`

Let's print some string first...

```python
print("Giraffe Academy")
```

We can add a "new line" inside a string with adding `\n`, something like this

```python
print("Giraffe\nAcademy")
```

Imagine we wanted to print quotes (`"`)  
We can't just put it there like so, because it will end the sentence we want to print  
But we can do so with `\"`  
The backslash (`\`) will make Python kind of ignore the quote (`"`), and it will treat it like a text instead of a special character

```python
print("Giraffe\"Academy")
```

We can also print variable with string, as we seen before

```python
phrase = "Giraffe Academy"

print(phrase)
print(phrase + "is cool")

```

> Note: Notice where I put quotes and where I did not
> If I wrote `print("phrase")`, the output will be `phrase` instead of `Giraffe Academy`

## String functions

Function is a little block of code that will do a specific operation for us  
Functions on strings can be used to modify given string

Print string in lowercase or uppercase

```python
print(phrase.lower())  # giraffe academy
print(phrase.upper())  # GIRAFFE ACADEMY
```

Check, if phrase is already uppercase  
It will give us [Boolean](#boolean) value whenever it is uppercase or not  
If we used the phrase from example above, it will give us `False`, because the whole phrase is not uppercase

```python
print(phrase.isupper())  # False
```

What will you get with following code?
If you said `True`, you are right  
First, the phrase gets "converted" to uppercase, then it checks if it's uppercase, which obviously is

```python
print(phrase.upper().isupper())  # True
```

With this code you can the the number of letters in the phrase  
`len` stands for "length", so basically you are looking for the length of the phrase

```python
print(len(phrase))  # 15
```

To get the first letter of phrase, you can do following  
Good thing to mention, **counting in programming usually starts with 0, not with 1**  
So if you want to get 1st character, you need to request the _0th_ character

```python
print(phrase[0])  # G
```

With this code, you can find the location (index) of given character  
If the phrase had multiple of given characters, it will return the location of first occurrence _(like in second line of code)_

```python
print(phrase.index("G"))  # 0
print(phrase.index("a"))  # 3
```

We can also look for more that one character

```python
print(phrase.index("Acad"))  # 8
```

If we were looking for a character or text, that is not present in phrase, we will get an error

```python
print(phrase.index("BOO"))  # *Error*
```

You can also replace a piece of string with another

```python
print(phrase.replace("Giraffe", "Elephant"))  # Elephant Academy
```

There are a lot more String function than these, I will stop it there for simplifying reasons

# Working with numbers

You can print numbers just like so

```python
print(3)
print(3.141592)
print(-2.9)
```

You can also put a math expression inside the print statement, Python will evaluate it

```python
print(4 + 3)  # 7
print(2 * 7)  # 14
print(15 / 3)
print(3 * (4 + 5)) # 27
```

A bit less know operator is modulus (`%`)  
It is the remainder after division  
If we had code like this...

```python
print(10 % 3)
```

...it will do `10 / 3`, which is equal to `3` and `1` is left, so it will return `1`

Take a look at some other examples

```python
print(12 % 5)  # 2
print(17 % 4)  # 1
print(10 % 2)  # 0
print(7 % 10)  # 7
```

It's a normal math operation, it's just less known

## Number functions

Similarly to [String funcitons](#String%20functions), also Numbers have functions

```python
print(abs(10))  # absolute value of 10 = 10
print(abs(-10))  # absolute value of -10 = 10
print(pow(3, 2)) # 3 to the power of 2 = 3^2 = 9

print(max(1, 9)) # prints the biggest of given numbers = 9
print(min(1, 9)) # prints the smalles of given nubmers = 1

print(round(3.14))  # rounds the number = 3
```

There are some more modules in the _math library_  
You don't need to worry what that means right now, you just need to import it at the beginning of file

```python
from math import *

print(floor(9.876))  # rounding to nearest smaller number = 9
print(ceil(1.234))  # rounding to nearest bigger number = 2

print(sqrt(36))  # square root = 6
```

# Getting input

Sometimes you want to get something from the user using your program  
You can do it with `input()` function, like this

```python
name = input("Enter your name: ")
```

User will enter his name and it will be stored in `name` variable

```python
name = input("Enter your name: ")
age = input("Enter your age: ")

print("Hello " + name + " you are " + str(age) + " years old")
```

You can also have empty `input()`, like the following code, but it may lead to confusion

```python
name = input()
```

You don't even have to store it inside a variable

```python
input()
```

# Basic calculator

A really basic calculator, which you can make with previous knowledge

```python
num1 = input("Enter a number: ")
num2 = input("Enter another number: ")
result = num1 + num2
print(result)
```

# MadLibs Game

```python
color = input("Enter a color: ")
noun = input("Enter a noun: ")
plural_noun = input("Enter a plural noum: ")

print("Roses are " + color)
print(plural_noun + " are blue")
print("I love " + noun)
```

# Lists

Lists in Python are basically a variables, which can store multiple values  
Lets make a list of friends for example

```python
friends = ["Kevin", "Karen", "Jim"]
```

Notice how the list is defined with square brackets `[]` and individual values are separated with comma `,`

You can print this list with `print(list_name)`  
This will print the list the same way as it was declared

```python
print(friends)  # ["Kevin", "Karen", "Jim"]
```

But sometimes we don't want to print the list like this  
Lets have a look how to access specific elements

## Accessing list elements

You can access a list element with `list_name[index_of_element]`  
As I mentioned before, **counting starts from 0** instead of 1  
That means that if we want to get first friend, we have to request a friend with the index of zero

```python
print(friends[0])  # Kevin
```

To print the last friend, we can use this cool feature

```python
print(friends[-1])  # Jim
```

If we wanted to access element that does not exist, we will get an error

```python
print(friends[999])  # *Error*
```

Now lets say we have a larger list

```python
friends = ["Kevin", "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"]
```

We want to print multiple elements, but not all of them  
We can use something called \_\_\_\_

```python
print(friends[1:4])  # "Karen", "Jim", "Oscar"
```

Previous code will print elements from index 1 _(2nd element = Karen)_ all the way to element with index 4, not including 4 (4th element = Oscar)

If I did the following, it will print all elements form the index of 1 to the end of this list  
We basically ignored the `end`, and by default it's set to the size of the list

```python
print(friends[1:])  # "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
```

If I ignored the first value, it will be set to `0` by default

You can add second semicolon with a number after, which will define _step_  
By default, the step is one, which means it will print the whole list  
You can set the step to `2` to print every second element, or to `3` to print every third element  
You can also print the list in reverse order by setting the step to `-1`

```python
print(friends[::2])  # every second = ['Kevin', 'Jim', 'Toby', 'Johny']
print(friends[::3])  # every third = ['Kevin', 'Oscar', Johny']
print(friends[::-1])  # reverse order = ['Ezhekiel', 'Johny', 'Max', 'Toby', 'Oscar', 'Jim', 'Karen', 'Kevin']
print(friends[::-2])  # in reverse order, every second = ['Ezhekiel', 'Max', 'Oscar', 'Karen']
```

Notice how I can ignore individual parts, and it will be set to the default value, resulting in printing the same list

```python
print(friends[0:7:1])  # "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
print(friends[0::1])  # "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
print(friends[::1])  # "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
print(friends[::])  # "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
```

## Change a value in list

Following code will replace the first value ("Karen") with "Peter"  
We can see this by printing the list

```python
friends[0] = "Peter"  # "Peter", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
```

## List functions

### `list.append(value)`

With following function we can add another value to the end of a list

```python
friends.append("Daniel")
print(friends)  # "Peter", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel", "Daniel"
```

### `list.insert(index, value)`

If we don't want to insert to the end of a list, but somewhere else, we can use `insert` function  
With following code, we set `Alex` to be the element with the index of 2 _(third element)_ and all the other will be pushed to the right

```python
friends.insert(2, "Alex") # "Peter", "Jim", "Alex", "Oscar", "Toby", "Max", "Johny", "Ezhekiel", "Daniel"
```

### `list.pop()`

`pop` is like an opposite function to `append`, it removes the last value

```python
friends.pop()
print(friends) # "Peter", "Jim", "Alex", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
```

### `list.remove(value)`

If we wanted to remove specific value from list, we can use `remove` function

```python
friends.remove("Alex")
print(friends) # "Peter", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
```

### `list.extend(another_list)`

If we had two lists and we will want to combine it to one, for whatever reason

```python
lucky_numbers = [3, 7, 9]
numbers = [10, 11, 12]

numbers.extend(lucky_numbers)
print(numbers)  # [10, 11, 12, 3, 7, 9]
```

We had the `numbers` list, and we basically added the `lucky_numbers` list to the end of it

### `list.index(element)`

Prints the index of given element in list

```python
print(friends.index("Oscar"))  # 3
print(friends.index("askdfha"))  # *Error*, because this value is not in a list
```

### `list.count(element)`

As the name suggests, `count` function will print the number of occurrences of element in list

```python
my_list = [1, 1, 1, 3, 4, 6]
print(my_list.count(1))  # 3
print(my_list.count("1"))  # 0 - we have numbers, not strings
print(my_list.count(6))  # 1
print(my_list.count(99)) # 0
```

### `list.sort()`

This will sort the list in alphabetical order

```python
friends.sort()
print(friends)  # ['Ezhekiel', 'Jim', 'Johny', 'Karen', 'Kevin', 'Max', 'Oscar', 'Toby']
```

If the list consists of numbers, it will sort the numbers

```python
my_list = [6, 5, 4, 7, 4, 9, 1, 6, 7]

my_list.sort()
print(my_list)  # [1, 4, 4, 5, 6, 6, 7, 7, 9]
```

### `list.reverse()`

This will reverse the list

```python
friends.reverse()
print(friends)  # ['Ezhekiel', 'Johny', 'Max', 'Toby', 'Oscar', 'Jim', 'Karen', 'Kevin']
```

### `new_list.copy(old_list)`

This will copy `old_list` into `new_list`  
So basically you will have two of the same lists

```python
new_friends = friends.copy()

print(friends)  # "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
print(new_friends)  # "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"
```

### `len(list)`

Do you remember the `len()` function from [String functions](#String%20functions)?  
It also works on lists  
By following code...

```python
print(len(friends))  # 7
```

...you will get the number of elements in the list

# Tuples

A Tuple in Python is something similar to List, but with one major difference  
Tuple can't be changed  
Once you define a tuple, it's elements cannot change

```python
my_tuple = (3.14, 9, 1.23456, "Hello")
```

Notice how tuple is defined with `()` whereas list was defined with `[]`

## Accessing Tuple elements

This works the same as in [Lists](#Accessing%20list%20elements)

```python
print(my_tuple[0])  # 3.14
```

The value of element or the number of elements cannot be changed

# Advanced Lists and Tuples

In Python, you can freely combine [Lists](#lists) and [Tuples](#tuples) together (and even [Dictionaries](#dictionaries), which will be covered later)  
In chapters before, the _type of element_ was either a Number, String, or Float  
We can totally put there a Boolean also, or even a List or Tuple as a value

## 2D List

### Creating 2D List

Lets make a List  
But instead of Numbers, it will be storing a Lists

```python
my_matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]

# you can split it in multiple lines, to make it look more organized
another_matrix = [
 [9, 8, 7],
 [6, 5, 4],
 [3, 2, 1]
]
```

Cool, right?  
We just created a "2D List"  
In fact, this is a List of Lists of Numbers

### Accessing 2D List elements

Remember how we accessed elements in normal List?  
We just typed `list[index]` and we got a Number  
But now, instead of Numbers, we have Lists there  
So what we do? It's as simple as `list[index][index]`  
With first index, we received a List, so we need to access it elements with another `[index]`

```python
print(my_matrix)  # [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(my_matrix[0])  # [1, 2, 3]
print(my_matrix[0][0])  # 1
```

## 3D List

We can also create 3D, 4D and so on...

```python
my_3d_list = [
 [
  [1, 2, 3], [4, 5, 6]
 ],
 [
  [7, 8, 9], [0, 1, 2]
 ],
 [
  [3, 4, 5], [6, 7, 8]
 ]
]

print(my_3d_list)  # [[[1, 2, 3], [4, 5, 6]], [[7, 8, 9], [0, 1, 2]], [[3, 4, 5], [6, 7, 8]]]
print(my_3d_list[0])  # [[1, 2, 3], [4, 5, 6]]
print(my_3d_list[0][1])  # [4, 5, 6]
print(my_3d_list[0][1][2])  # 6
```

3D list is technically a List of Lists of Lists of Numbers  
You see that it can get pretty messy if I put everything in one line, that's why I prefer writing it to multiple lines, even if it's not required

## Combining structures

We can also put a Tuples inside of Lists or Lists inside of Tuples

```python
coordinates = [
 (3, 4), (6, 2), (80, 64)
]

points = (
 [2, 4, 6, 12],
 [7, 5, 6, 2],
 [4, 6, 0, 0]
)
```

Accessing elements from those should be pretty straight forward, we already did this

```python
print(coordinates)  # [(3, 4), (6, 2), (80, 64)]
print(coordinates[0])  # (3, 4)
print(coordinates[0][1])  # 4

print(points)  # ([2, 4, 6, 12], [7, 5, 6, 2], [4, 6, 0, 0])
print(points[1])  # [7, 5, 6, 2]
print(points[1][3])  # 2
```

This way you can create any structure you need  
Just remember that Tuples cannot be changed, so once you have a Tuple, it's there forever

# Functions

## Basic stuff about Functions

Lets say you have a piece of code, which you wanted to execute multiple times

```python
my_list = []

my_list.append(1)
print(my_list)

my_list.append(1)
print(my_list)

my_list.append(1)
print(my_list)
```

Now the code gets pretty big  
And imagine if you wanted to execute like 300 lines of code multiple times  
...Now you changed your mind and you want to do something different

Good solution for this is a _function_  
Let's define a function to see what I am talking about  
Use `def function_name():` to define a function

```python
def do_something_with_list():
```

Now we kind of made a function, but we will get error because it's empty  
The code you want to put inside a function must be _indented_ with `tab` key on your keyboard

```python
def do_something_with_list():
 print("Running function...")
 my_list.append(1)
 print("Your list just got extended")
 print("I will print the list in the line bellow")
 print(my_list)
 print("Function done...")
```

When you run the code right now, it seems like nothing happened  
Why? Because, actually, nothing happened  
We never told the function to run - we did never **call the function**  
We need to call the function in order to run  
It's as easy as writing it's name along with parenthesis, like in example bellow

```python
do_something_with_list()
```

Now we called the function and you can see all the text we defined in the function

We can call the function multiple times, you can see how the list expands

```python
do_something_with_list()
do_something_with_list()
do_something_with_list()
do_something_with_list()
do_something_with_list()
```

Now we saved a lot of work huh?  
It can be even shorter, you will see in a while

**Order matters**  
I you wanted to call a function before it was defined, you will get an error

```python
do_something()  # *Error*


def do_something():
 print("Trying not to look useless")


do_something()  # this will run, if the first line wasnt there
```

## Function parameters

In the previous function, we added `1` to the end of a list every time  
What if we wanted to add a different number every time

We can add something called _parameter_ to the function definition  
I'm pretty sure you noticed those parentheses in function definition  
The parameter name is written in between the parenthesis, like in example bellow

```python
def do_something_with_list(number_to_add):
 ...
```

Now we just created a kind of variable, that can be used only inside the function  
Now we can adjust the function, to add out desired number, not just `1`

```python
def do_something_with_list(number_to_add):
 print("Running function...")
 my_list.append(number_to_add)
 print("Your list just got extended with " + str(number_to_add) + ". The whole list now looks like this: ")
 print(my_list)
 print("Function done...")
```

When calling function with parameters, we _have to_ write those parameters there

```python
do_something_with_list(1)
do_something_with_list(5)
do_something_with_list(3)
do_something_with_list(6)
do_something_with_list(7)
```

We can add as many parameters as we want

```python
def say_hi(name, age):
 print("Hello " + name + ", you are " + str(age) + " years old")


say_hi("Peter", 21)  # Hello Peter, you are 21 years old
say_hi("Adka", 15)  # Hello Adka, you are 15 years old
say_hi("Jarko", 10)  # Hello Jarko, you are 10 years old
```

If we had some variable in our code, we can freely pass it to function as it's parameter

```python
my_name = "Peter"
my_age = 21


def say_hi(name, age):
 print("Hello " + name + ", you are " + str(age) + " years old")


say_hi(my_name, my_age)
```

## Return statement

Maybe sometimes in code, we don't want a function print anything  
We might want the function just to do something or calculate something, so we can use the value later on

Let's say that we want to calculate the volume of a cube

> Note: Remember that we _don't want to print_ the value right now

```python
def cube_volume(size):
 size * size * size


cube_volume(5)  # *This won't print anything*
print(cube_volume(5))  # None
```

This function takes one argument - size of a cube - and calculates it's volume  
Calling the function will seemingly do nothing  
In fact, the function did run, but we didn't told it what to do with the result

We can tell it to **run the function, and give us the value** by using `return`

```python
def cube_volume(size):
 return size * size * size


cube_volume(5)  # *This won't print anything once again*
print(cube_volume(5))  # 125
```

There you have it!  
The function ran, and _returned_ 125  
You can imagine it like the `cube_volume(5)` is being replaced by 125, which is the `return` value

We can use this more...

```python
def cube_volume(size):
 return size * size * size


print("The cube with size of " + str(3) + " has a volume of " + str(cube_volume(3)))  # The cube...3...volume of 27
print("The cube with size of " + str(4) + " has a volume of " + str(cube_volume(4)))  # The cube...4...volume of 64
print("The cube with size of " + str(2 + 5) + " has a volume of " + str(cube_volume(2 + 5)))  # The cube...7...volume of 343

print("Both cubes together have the volume of " + str(cube_volume(3) + cube_volume(4)))  # ...volume of 91
print("If we subtracted the volumes, we will get " + str(cube_volume(4) - cube_volume(3)))  # ...we will get 37

```

As you might see, we can use even math operations as parameters

You can also see the versatility of the function this way. If you put the `print` statement directly inside the function, you will always get the same result  
If you wanted to print something different, you will have to create a new function

The code in function after `return` will not get executed

```python
def second_power(num):
 return num * num
 print("Something something")  # this will not be printed, because it's in code after return
```

# `if`, `else`, `elif`

Now we will talk a bit about something called _conditions_  
It basically goes like `if something happens, do something else`  
Some code might or might not get executed, based on given conditions

## `if`

Imagine a little story...

```text
I wake up
If I'm hungry, I eat breakfast

I leave my home
If it's cloudy, I bring and umbrella
Otherwise, I bring sunglasses

Im at a restaurant
If I want a meat, I order a steak
Otherwise, If I want pasta, I order spaghetti and meatballs
Otherwise, I order salad
```

You can make this in Python really easily

```python
i_am_hungry = True

if (i_am_hungry == True):
 # the code bellow will get executed, because the variable is True
 print("I want to eat a breakfast")

# now if we changed the variable to false...
i_am_hungry = False

if (i_am_hungry == True):
 # this will not get executed, because the the condition above is no longer true
 print("I want to eat a breakfast")

```

> Note: Notice the difference between `=` and `==` > `=` is an assignment operator. It's used when you are assigning a value to variable or something similar
> `==` is an comparison operator. It's used when you are comparing if two values are the same

The code inside the `if` statement will get executed only if the condition inside bracket is valid  
In the second half of code, the condition is not valid, so the code inside will not get executed

Imagine like `i_am_hungry` - the variable - is replaced with it's value  
So basically Python sees it like this `if (True == True): ...`  
It's comparing these two values, and the comparison is valid, meaning the whole condition is valid

You can also compare numbers and strings, not only boolean

```python
if (age < 18):
 print("You can't drink alcohol")

if (name == "Peter"):
 print("Welcome back!")
```

### Logical operators

#### `and`

The logical and in Python is just `and`  
Imagine if you have some kind of login system, and you want to check if the username _and_ password is correct

```python
if (name == "Peter" and password == "Fialka123"):
 print("Log in success, welcome!")
```

The code inside `if` statement will be executed only if **both** are `True`

#### `or`

Similarly to previous example, logical or is just `or` in Python

```python
if (weather == "rain" or weather == "thunderstorm"):
 print("I should take an umbrella")
```

#### `not`

If you wanted a logical `not`, you can do it like this...

```python
if (name != "Peter"):
 print("Unauthorized access!")
```

`!` is logical not, which like reverses the value

You can do whole lot of stuff combining these logical operators and `if` conditions

### `if` shorthands

There are actually 2 shorthands to save you some time typing

#### Boolean shorthands

`if` statement is looking inside the parentheses, to decide whenever is the condition true or not  
`i_am_hungry == True` is translated to `True == True`, which is translated to `True`  
So basically, if we did this...

```python
if (i_am_hungry): ...
```

...it will have the same effect as the code before, because `i_am_hungry` will be translated to `True`  
This only makes sense if you are using a boolean value inside the `if` condition

But what if we had the code written like this...

```python
if (i_am_hungry == False): ...
```

You can use `not`, like this...

```python
if (not i_am_hungry): ...
```

#### Brackets shorthads

You don't have to use brackets in `if` statements  
I used them, just for it to make more sense, but Python does not require it  
Conditions can look like this

```python
if i_am_hungry: ...

if not name == "Peter": ...
```

## `else`

`else` goes something like this: _If `if` statement is not valid, execute this code instead_  
Maybe you'll understand better if i gave you some examples

```python
if is_male:
 print("You are a male")
else:
 print("You are a female")
```

```python
if age > 18:
 print("You are allowed to drink alcohol")
else:
 print("You can't drink yet")
```

```python
if i_am_hungry:
 print("I need something to eat right now")
else:
 print("I will eat later")
```

> Note: Notice the indentation

## `elif`

Let's go back to the story from [if chapter](#`if`), especially the last part

```python
i_want_meat = False
i_want_pasta = False


print("I'm at restaurant")

if i_want_meat:
 print("I will order a steak")
else:
 if i_want_pasta:
  print("I will order spaghetti and meatballs")
 else:
  print("I will order a salad")

```

> Note: Notice the indentation  
> There is another `if` statement under `else`  
> We want the whole second `if` execute only if the first `if` is not valid

Now this will work just fine, but it's a bit impractical  
If we had, let's say 20 of these _nested `if`'s_, we will have to indent if 20 times and we might get lost in it over time

There comes in handy `elif`  
It stands for `else if`

Notice this part in previous code

```python
...
else:
 if i_want_pasta:
...
```

The `else` and `if` can be replaced with easily `elif`, like this

```python

if i_want_meat:
 print("I will order a steak")
elif i_want_pasta:
 print("I will order spaghetti and meatballs")
else:
 print("I will order a salad")
```

This is used when first _(or second, third, ...)_ condition is not valid and you want to check for another condition

If we had some kind of login system...

```python
if username == "Peter":
 if password == "Fialka123":
  print("Login success, welcome!")
 elif password == "Pampeliska123" or password == "Tulipan123:
  print("You were close, try again")
 elif password == "12345678":
  print("I'm not that dumb")
 else:
  print("Password incorrect, try again...")
else:
 print("Username not found")


if "dog" == "dog":
 print("ruff ruff")
elif "dog" != "dog":
 print("something ain't right")
else:
 print("meow")
```

This is also a good example on _nested `if`'s_ and like an overall summary of this chapter

You can also use `if`'s inside functions

```python
def get_largest_number(a, b, c):
 if a > b and a > c:
  return a
 elif b > a and b > c:
  return b
 else:
  return c


print(get_largest_number(1, 2, 3))  # 3
print(get_largest_number(5, 23, 1))  # 23
print(get_largest_number(6, 0, -100))  # 6
```

# Better Calculator

Now we know a bit more, let's make a better calculator

```python
def calculate(num1, operator, num2):
 if operator == "+":
  return num1 + num2
 elif operator == "-":
     return num1 - num2
 elif operator == "*":
     return num1 * num2
 elif operator == "/":
     return num1 / num2
 else:
  print("You have entered invalid operator...")

num1 = float(input("Enter first number: "))
operator = input("Enter operator (+ - * /): ")
num2 = float(input("Enter second number: "))

print("Result of your calculation is: " + str(calculate(num1, operator, num2)))
```

> Note: You might see `float()` for the first time  
> Up until now, we converted `number` into `string` inside print statements  
> This works the same, but with float, input gets converted into a number with decimal part

# Dictionaries

Dictionary is another way of storing multiple values in variable, just like [Lists](#lists) and [Tuples](#tuples)  
While List looked like this: `[value, value, value,...]`, Dictionary looks like this: `{key: value, key: value, key: value,...}`

In List, we are storing a `value`, which can be accessed with it's `index`  
In Dictionary, we are storing a `value` which can be accessed with it's `key`

In List, `index` is a `number`  
In Dictionary, `key` can be _any type_

Let's take a look on actual Dictionary

```python
month_conversions = {
    "Jan": "January",
    "Feb": "February",
    "Mar": "March",
    "Apr": "April",
    "May": "May",
    "Jun": "June",
    "Jul": "July",
    "Aug": "August",
    "Sep": "September",
    "Oct": "October",
    "Nov": "November",
    "Dec": "December",
}
```

> Note: Notice how values are also separated by comma (`,`) (same as in List)  
> Notice how Dictionary is defined with curly brackets `{}` (Unlike parentheses `()` in List)  
> Notice that there is a semicolon (`:`) between `key` and `value`

To access Dictionary values...

```python
print(month_conversions["May"])
```

Another example of Dictionary

```python
english_to_slovak = {
 "Apple": "Jablko",
 "Banana": "Banan",
 "Orange": "Pomaranc"
}

print(english_to_slovak["Apple"])
```

`key` and `value` in dictionary doesn't have to be just `string`

```python
list_of_age = {
 "Peter" = 21,
 "Johan" = 999,
 "Ferko" = 2
}

print(list_of_age["Peter"])
```

```python
scored_points = {
 "Peter" = [4, 2, 6, 1],
 "Johan" = [13, 5, 2, 5],
 "Ferko" = [0, 0, 1, 0]
}

print(scored_points["Ferko"])
print(scored_points["Ferko"][2])
```

# While loops

`while` loop is one of two loops in Python  
`while` loops are meant to iterate, while condition is true  
The logic behind `while` loop is something like this: `while something is true, do something`

The structure of `while` loop looks like this:

```python
while <condition>:
 <do_something>
```

```python
x = 1

while x < 10:
 print("x is now equal to " + str(x))
 x = x + 1  # incrementing i


```

If you run the code before, you might see the `x` variable increasing  
_While_ is `x` less than `10`, it writes the sentence and increases `x` _(this is basically what the code does)_  
It does something, while something else is true

Another example...

```python
entered_password = ""

while entered_password != "Fialka123":
 entered_password = input("Enter password: ")
```

This basically asks for password, until you enter `Fialka123`

## Increment shorthand

In the code before, you might see something like `x = x + 1`  
This basically takes `x` and adds `1` to it  
If `x` was 8 for example, it will look like this: `x = 8 + 1`

There is a shorthand for this

```python
x += 1
```

This same can be done for all operations

```python
x = x + 1
x += 1

y = y - 1
y -= 1

z = z * 2
z *= 2

w = w / 2
w /= 2
```

## `break`

If you put `break` somewhere inside `while` loop, the whole loop will end  
It's like a way for it to say "stop doing everything, right now"  
The `while` loop will intermediately stop, and the program will resume with the code after the loop

```python
user_input = ""

while not user_input == "Hello":
 user_input = input("Enter a phrase: ")

 if user_input == "stop":
  break
```

In this piece of code, we are telling the loop to _break_ when the user types in "stop"

## `continue`

If you put `continue` somewhere inside `while` loop, the current iteration will skip  
It's like saying "continue with next value"  
It's meant for like solving special cases I would say

```python
x = 1

while x <= 10:
 if x == 4 or x == 5:
  continue

 print(str(x) + " is my favorite number")

```

This will print all the numbers from `1` to `10`, instead of `4` and `5`, because those are not your favorite numbers ;)

---

You can take another look on `break` and `continue` in [for loops section](#`break`%20&%20`continue`%20in%20`for`%20loops)

# Guessing game

Lets take a look on some kind of practical example  
We are going to make a simple game, where the computer gives you a _random_ word and you have to guess it  
You will have 3 guesses  
If you'll manage to guess the word, you will win, otherwise you'll lose

> Note: For now, the word wouldn't be random, to make things easier

```python
secret_word = "Giraffe"
player_won = False

guesses_left = 3

while guesses_left > 0:
    guessed_word = input("Guess the secret word: ")

    if guessed_word == secret_word:
        player_won = True
        break

    guesses_left -= 1

if player_won:
    print("Congratulations, you won!")
else:
    print("You didn't guessed the right word, sorry... The word you were looking for was " + secret_word)
```

There are a lot of ways to program this, but this is one of the example

If you wondered, how we could actually make the word random...  
You can add some words to a List  
You can store random value from the list to the variable `secret_word`  
You will learn later, how to use `random` in Python

# For loops

`for` loop in Python is another type of loop, some kind of similar to `while` loop  
While `while` loops are meant to iterate, while condition is true, `for` loops are meant to iterate over sequence  
The logic behind `for` loop is something like this: `do something this amount of times`  
Or maybe this will be better: `for every element of sequence, do this`

The structure of `for` loops looks like this:

```python
for <variable_name> in <sequence>:
 <do_something>
```

```python
for letter in "Giraffe Academy":
 print(letter)
```

The code before will print the word `Giraffe Academy` letter by letter, each one in new line

You can loop through List like this:

```python
friends = ["Jim", "Karen", "Kevin"]

for f in friends:
 print(f)
```

This way, each element of the list will be printed one by one, each one in new line

## `for` loops with `range()`

Another common use of `for` loops, is when you want to do something specific amount of times  
For this, you can use `range()` function

```python
for i in range(10):
 print(i)
```

It might not work exactly the way you expect...  
After running this, you will see the numbers from `0` to `9`

### `len()` function

Do you remember the `len()` function from [List functions](<#`len(list)`>)  
By combining `len(list)` and `range()`, you can do some cool stuff

```python
racers = ["Max", "Lando", "Charles", "Oscar", "Carlos"]

print("Racers standings: ")
for i in range(len(racers)):
 print(str(i + 1) + ". " + racers[i])
```

Remember that now we are looping through numbers from `0` to (_size of a list - 1_) which is `4`

## `break` & `continue` in `for` loops

You can also use [`break`](#`break`) and [`continue`](#`continue`) within for loop

```python
friends = ["Kevin", "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"]

for f in friends:
 if f == "Jim":
  continue

 if f == "Max":
  break

 print("Hello " + f)
```

This code will print all friends, but:

- It will not print for `Jim`
- Once it reaches `Max`, the loops ends
  - Therefore it won't print for `Max`, `Johny` and `Ezhekiel`

## Nested `for` loops and 2D Lists

With nested `for` loops, you can iterate through 2D List

```python
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
]

print(number[0][0])  # number on row 0 and column 0 == 1

for row in number_grid:
    for col in row:
        print(col)
```

# Simple Translator

We are going to build a simple translator, which translates given phrase with one special rule...  
Vowels are going to be translated to `g`  
So for example, `dog` becomes `dgg`, `coconut` becomes `cgcgngt` and so on, you got the point

```python
def translate(phrase):
    result = ""
    vowels = ["a", "e", "i", "o", "u"]

    for letter in phrase:
        if letter in vowels:
            result += "g"
        else:
            result += letter

    return result


user_input = input("Enter a phrase to translate: ")
print(translate(user_input))
```

You can expand it a bit, so it also recognizes capital letters

```python
def translate(phrase):
    result = ""
    vowels = ["a", "e", "i", "o", "u", "A", "E", "I", "O", "U"]

    for letter in phrase:
        if letter in vowels:
            if letter.isupper():
                result += "G"
            else:
                result += "g"
        else:
            result += letter

    return result

print(translate(input("Enter a phrase to translate: ")))
```

You could also create two Lists with uppercase and lowercase letters separated  
Notice how I simplified the last line

# `try-except`

It definitely happened to you, you ran a code and you got some kind of error  
Error popped out, and the code ended immediately  
There is kind of a way around it, with `try` and `except` in Python  
It basically goes like: `try to execute this, if error (exception) happens, do this`

There is an example, when user was supposed to enter a `number`, but entered a `string` instead  
Python tried to convert `string` to `number` and failed, so you got an error (exception)

```python
user_input = int(input("Enter a number: "))  # if i entered a string instead of number, the program will crash
```

With `try` and `except`, there is a way around it

```python
try:
 user_input = int(input("Enter a number: "))
 print("Hello there")
except:
 print("You probably didn't enter a number...")
```

The program will not crash, and the code after will continue to execute normally  
If the input is OK, it will print `Hello There`  
If the input is not OK, it will print `You probably didn't enter a number...`

Notice the `print("Hello there")` after the input  
I wanted to show that **if error happens, it immediately goes to `except` block, without executing next code**  
This will be important in next code, so keep that in mind

There is a bit problem to this specific situation, you will not get the actual input from user  
You can fix it simply with infinite `while` loop  
Adding a `break` after the line with input will cancel the loop and continue to execute code after the `while-try-except`

```python
while True:
    try:
        user_input = int(input("Enter a number: "))
        break
    except:
        print("You probably didn't enter a number...")
...
```

So once again, how does this work?

- `while True: ...` is an infinite while loop. It will execute the code inside forever. One way to get out of it is using `break`
- `try: ...` will try to execute the code inside. If there is an error, it immediately goes to `except`. In this case it does following:
  - Tries to get input from user. There are only two possible scenarios:
    - There is an error - code inside `except` gets executed
    - There isn't any error - code continues. The next code is `break`, which will end the infinite `while`. What comes after `while`? Next code. In this example it's illustrated with three dots (`...`)
- `except: ...` The code inside will get executed when some kind of error (exception) happens

# Working with files

Wouldn't it be cool to work with `.txt`, `.csv`, `.json` and similar files?

Before continuing, remember two things

- Before working with a file, you have to `open` it
- After working with a file, you have to `close` it

> Note: After closing the file, you will no longer be able to work with it in your Python script, unless opened again  
> It tells other programs and parts of the OS that the file is ready to work with again

## Reading from files

Reading from file is an operation, where you have a file stored on you drive, and you want to get some information from it  
With following code, we are reading the file `employees.txt` and storing it inside `employee` variable

```python
file = open("employees.txt", "r")
file.close()
```

Inside the `open()` function, we passed two parameters

- `emplyees.txt` - name of the file - we want to open file called `employees.txt` which is located in the same folder as the Python script
- `r` - mode - `r` stands for "read", meaning that we want only to load the content of the file and we don't want to modify it

Remember that this is not the actual content of the file, just the file itself  
If we printed the file if could look something like this: `<_io.TextIOWrapper name='file.txt' mode='r' encoding='UTF-8'>`

To actually read the content of the file, we have to use the `read()` function

```python
file = open("employees.txt", "r")

print(file.readable())  # check, if the file is readable
print(file.read())  # reading all the content from the file

file.close()
```

> Note: Don't forget to close the file

With `file.read()` you just printed the whole contend of the file at once  
Better approach might be `file.readlines()`  
It reads the file line by line, and stores it into a List  
Each element of this List is a line of the file  
With this you can loop through the list and do various stuff

```python
file = open("employees.txt", "r")

content = file.readlines()

for line in content:
 print(line)

file.close()
```

Remember that it puts a newline symbol (`\n`) at the end of every line

## Writing to files

Writing to file is an operation, where you change the content of file  
First, you need to open the file, as stated in previous part, then you can write to it  
And don't forget to close it at the end :)

### `w` mode

```python
file = open("some_text_file.txt", "w")

file.write("Hello there, this text was written using Python")

file.close()
```

With setting `w` as the second parameter of `open()` function, we opened the file in `write` mode and thus make it able to **rewrite** the file  
**Be careful!** You will **rewrite** all the content in it, meaning that all the original content is gone forever!  
Think twice before you rewrite something, it's not possible to get it back  
It's a good practice to not open the file in `write` mode if not necessary

### `a` mode

If you want a bit safer option, you can open the file in `append` mode  
If you pass `a` as the second parameter, you will open the file in `append` mode, which will only **add something to the end of the file**  
It will keep the original content, and something more to it

```python
file = open("favorite_words", "a")

file.write("Hello")

file.close()
```

As said before, this will only add to the file, leaving the original content untouched  
If we ran the Python script 5 times, the file will look like this: `HelloHelloHelloHelloHello`  
Notice how we didn't put `\n` anywhere, so it's the word 5 times in one line (as expected)

## `with open()`

It might happen to you that you forgot to close the file when you no longer needed it  
Python has this really cool feature  
`with open()` block allows you to work with file, and automatically close it at the end

```python
with open("employees.txt", "r") as file:
 print("file.read()")
```

You just opened the `employees.txt` file in `read` mode, and stored it into variable called `file`, just as we did in the first example of the beginning of this chapter  
This helps you to save some time typing  
As stated before, you don't have to put `file.close()` at the end, because it closes automatically

# `import`

With `import` in Python, you can use parts of other files/modules/libraries in your script

Lets say we have two Python files...
One called `useful_stuff.py` where we will be storing a _really useful stuff_...

```python
# file: userful_stuff.py
beatles = ["John Lennon", "Paul McCartney", "George Harrison", "Ringo Star"]

def greeting():
 print("Hello there! Welcome back, I hope you had a nice day :)")
```

...and another one called `main.py`, where will be like the main thing we want to do. Only this file will get executed

```python
#file: main.py
import useful_stuff

print(useful_stuff.beatles)

useful_stuff.greeting()
```

With `import` we make it able to use another file in this file

> Notice how I didn't use `.py` in `import`
> Notice how did I access the stuff from `useful_stuff` - using a dot (`.`)

If you don't want to use the name `useful_stuff`, you can change it like this:

```python
import useful_stuff as file

print(file.beatles)
```

If you don't want to import and use the whole file, you can import just part of it like this:

```python
from useful_stuff import beatles

print(beatles)
```

...now we don't have other stuff from `useful_stuff`, but the program will be lighter to run

You can also combine these if you want...

```python
from useful_stuff import greeting as hello

hello()
```

> Notice how I used parentheses, because it's a function

## Importing libraries/modules

Python has some really useful libraries/modules, which you can import

```python
import random

print(random.randint(1, 20))
```

With this code, we generated a random number ranging from `1` to `20`

It can be used like this for example...

```python
import random

def roll_the_dice():
 return random.randint(1, 6)
```

You can choose randomly from a list like this

```python
friends = ["Kevin", "Karen", "Jim", "Oscar", "Toby", "Max", "Johny", "Ezhekiel"]

print(random.choice(friends))
```

There is a lot more to `random` and there are a lot more libraries/modules

# Classes

I can't really explain Classes, but I will try my best  
A `class` can be compared to object in real life  
We can create `class` to represent an real-life object

For example, a person  
Each person has a name, surname, age, date of birth and whatever else  
Up until now, we stored this information as following

```python
my_name = "Peter"
my_surname = "Cyprich"
my_age = 21
```

If we have only one person, it's not that bad  
But imagine you wanted to store this data about a 100 people  
You will have to create 300 variables, which is not very practical, right?

In situations like this, `class`'es comes in handy  
You can make a `class`, which will represent a person

> Note: You might not understand all the code right now, we will get into it in just a moment

```python
class Person:
 __init__(self, name, surname, age):
  self.name = name
  self.surname = surname
  self.age = age

 def print_name(self):
  print(self.name)

 def print_surname(self):
  print(self.surname)

 def print_age(self):
  print(self.age)
```

Right now we defined a Person class

> Note: Notice how the class name starts with capital letter  
> Notice how every _attribute_ (like a variable of a class) has `self.` in front

Each class has it's methods, in this case they are `__init__()`, `print_name()`, `print_surname()` and `print_age()`  
I think that the `print_name()` and following classes are pretty obvious - when called, it will print corresponding variable

> Note: If you wonder how to call a function like this, I will get into it in a moment

But what about the `__init__()` method?  
This is called a **constructor**  
It's used to _construct_ = create an object  
It's just a method, which when called, will create and return an object from class  
In this case, when we will call the constructor, it will create a new Person

Now there are 4 parameters to the constructor: `self`, `name`, `surname`, `age`  
Ignore the `self` for now, lets focus on the others...  
Values to these parameters have to be passes to the constructor when calling it  
To call the constructor we just type the name of the class, in this case it's `Person`

```python
myself = Person("Peter", "Cyprich", "Age")
```

With this code, we just created a new Person, and stored it inside `myself` variable

Now we can use this variable, and access all the variables

```python
myself.print_name()  # Peter
myself.print_surname()  # Cyprich
myself.print_age()  # 21
```

Pretty cool right?  
We can create as much _instances_ of this class and use them

```python
brother = Person("Jozko", "Ferko", 999)
sister = Person("Ferka", "Jozka", 2)
friend = Person("Juraj", "Janosik", 30)

sister.print_name()  # Ferka
brother.print_age()  # 999
friend.print_surname()  # Janosik
```

# venv

Virtual environment

```bash
python -m venv my-env  # create venv
source my-env/bin/activate  # activate venv
deactivate  # deactivate venv

pip install raylib  # install packages
pip freeze > requirements.txt  # save dependencies
pip install -r requirements.txt  # install dependencies
```

# dotenv

Enviromental files
You should always avoid putting sensitive info (passwords) directyly in your code
One way to fix this is with `python-dotenv` library

First you need to install the `python-dotenv` library

```bash
pip install python-dotenv
```

Now create `.env` file in your project, and put your sensitive info there

```env
USERNAME=myuser
PASSWORD=strongpassword
EMAIL=myuser@myuser.myuser
```

To access it...

```python
import os
from dotenv import load_dotenv

load_dotenv()

print(os.getenv("USERNAME"))
print(os.getenv("PASSWORD"))
print(os.getenv("EMAIL"))
```

**If you are using git, don't forget to add `.env` to `.gitignore` !!!**

---

# Various stuff

## Modules

`dir()`

```python
import math
print(dir(math))  # prints all functions of math module
```

`platform` module

```python
import platform

print(platform.platform())  # Linux-6.12.1-arch1-1-x86_64-with-glibc2.40
print(platform.machine())  # x86_64
print(platform.uname())  # uname_result(system='Linux', node='HP-Victus-Endeavour', release='6.12.1-arch1-1', version='#1 SMP PREEMPT_DYNAMIC Fri, 22 Nov 2024 16:04:27 +0000', machine='x86_64')
print(platform.system())  # Linux
print(platform.version())  # #1 SMP PREEMPT_DYNAMIC Fri, 22 Nov 2024 16:04:27 +0000
print(platform.architecture())  # ('64bit', 'ELF')
# print(platform.processor())  #
print(platform.python_version())  # 3.12.7
print(platform.python_implementation())  # CPython
print(platform.python_build())  # ('main', 'Oct  1 2024 11:15:50')
# print(platform.python_revision())  #
print(platform.python_compiler())  # GCC 14.2.1 20240910
```

## String/List methods

`ord()` method on character _(ordinal)_  
returns ascii code of given character

```python
print(ord('a'))  # 97
print(ord(' '))  # 32
```

`chr()` mathod is the opposite of `ord()` - returns a symbol by its number in ascii table

```python
print(chr(97))  # 'a'
print(chr(98))  # 'b'
print(chr(99))  # 'c'
```

`center()` method on string

```python
print("|" + "alpha.center(11)" + "|")  # |   alpha   |

print('[' + 'Beta'.center(2) + ']')  # [Beta]
print('[' + 'Beta'.center(4) + ']')  # [Beta]
print('[' + 'Beta'.center(6) + ']')  # [ Beta ]

print("|" + "alpha.center(9, "*")" + "|")  # |**alpha**|
```

`join()` method on string

```python
print(",".join(["omicron", "pi", "rho"]))  # omicron,pi,rho
```

## Useful Exception

| Name                | Decription                                                                                                                                                                                                                               |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ArithmeticError`   | An abstract exception including all exceptions caused by arithmetic operations like zero division or an argument's invalid domain                                                                                                        |
| `AssertionError`    | \*\*discussed later\*\*                                                                                                                                                                                                                  |
| `BaseException`     | The most general exception                                                                                                                                                                                                               |
| `IndexError`        | A concrete exception raised when you try to access a non-existent sequence's element (e.g., a list's element)                                                                                                                            |
| `KeyboardInterrupt` | A concrete exception raised when the user uses a keyboard shortcut designed to terminate a program's execution (Ctrl-C in most OSs); if handling this exception doesn't lead to program termination, the program continues its execution |
| `LookupError`       | An abstract exception including all exceptions caused by errors resulting from invalid references to different collections (lists, dictionaries, tuples, etc.)                                                                           |
| `MemoryError`       | A concrete exception raised when an operation cannot be completed due to a lack of free memory                                                                                                                                           |
| `OverflowError`     | A concrete exception raised when an operation produces a number too big to be successfully stored                                                                                                                                        |
| `ImportError`       | A concrete exception raised when an import operation fails                                                                                                                                                                               |
| `KeyError`          | A concrete exception raised when you try to access a non-existent element in a collection (e.g., a dictionary's element)                                                                                                                 |

All the exceptions are like this

```
BaseException
    +---Exception
    |    +---TypeError
    |    +---StopAsyncIteration
    |    +---StopIteration
    |    +---ImportError
    |    |    +---ModuleNotFoundError
    |    |    +---ZipImportError
    |    +---OSError
    |    |    +---ConnectionError
    |    |    |    +---BrokenPipeError
    |    |    |    +---ConnectionAbortedError
    |    |    |    +---ConnectionRefusedError
    |    |    |    +---ConnectionResetError
    |    |    +---BlockingIOError
    |    |    +---ChildProcessError
    |    |    +---FileExistsError
    |    |    +---FileNotFoundError
    |    |    +---IsADirectoryError
    |    |    +---NotADirectoryError
    |    |    +---InterruptedError
    |    |    +---PermissionError
    |    |    +---ProcessLookupError
    |    |    +---TimeoutError
    |    |    +---UnsupportedOperation
    |    |    +---herror
    |    |    +---gaierror
    |    |    +---timeout
    |    |    +---Error
    |    |    |    +---SameFileError
    |    |    +---SpecialFileError
    |    |    +---ExecError
    |    |    +---ReadError
    |    +---EOFError
    |    +---RuntimeError
    |    |    +---RecursionError
    |    |    +---NotImplementedError
    |    |    +---_DeadlockError
    |    |    +---BrokenBarrierError
    |    +---NameError
    |    |    +---UnboundLocalError
    |    +---AttributeError
    |    +---SyntaxError
    |    |    +---IndentationError
    |    |    |    +---TabError
    |    +---LookupError
    |    |    +---IndexError
    |    |    +---KeyError
    |    |    +---CodecRegistryError
    |    +---ValueError
    |    |    +---UnicodeError
    |    |    |    +---UnicodeEncodeError
    |    |    |    +---UnicodeDecodeError
    |    |    |    +---UnicodeTranslateError
    |    |    +---UnsupportedOperation
    |    +---AssertionError
    |    +---ArithmeticError
    |    |    +---FloatingPointError
    |    |    +---OverflowError
    |    |    +---ZeroDivisionError
    |    +---SystemError
    |    |    +---CodecRegistryError
    |    +---ReferenceError
    |    +---BufferError
    |    +---MemoryError
    |    +---Warning
    |    |    +---UserWarning
    |    |    +---DeprecationWarning
    |    |    +---PendingDeprecationWarning
    |    |    +---SyntaxWarning
    |    |    +---RuntimeWarning
    |    |    +---FutureWarning
    |    |    +---ImportWarning
    |    |    +---UnicodeWarning
    |    |    +---BytesWarning
    |    |    +---ResourceWarning
    |    +---error
    |    +---Verbose
    |    +---Error
    |    +---TokenError
    |    +---StopTokenizing
    |    +---Empty
    |    +---Full
    |    +---_OptionError
    |    +---TclError
    |    +---SubprocessError
    |    |    +---CalledProcessError
    |    |    +---TimeoutExpired
    |    +---Error
    |    |    +---NoSectionError
    |    |    +---DuplicateSectionError
    |    |    +---DuplicateOptionError
    |    |    +---NoOptionError
    |    |    +---InterpolationError
    |    |    |    +---InterpolationMissingOptionError
    |    |    |    +---InterpolationSyntaxError
    |    |    |    +---InterpolationDepthError
    |    |    +---ParsingError
    |    |    |    +---MissingSectionHeaderError
    |    +---InvalidConfigType
    |    +---InvalidConfigSet
    |    +---InvalidFgBg
    |    +---InvalidTheme
    |    +---EndOfBlock
    |    +---BdbQuit
    |    +---error
    |    +---_Stop
    |    +---PickleError
    |    |    +---PicklingError
    |    |    +---UnpicklingError
    |    +---_GiveupOnSendfile
    |    +---error
    |    +---LZMAError
    |    +---RegistryError
    |    +---ErrorDuringImport
    +---GeneratorExit
    +---SystemExit
    +---KeyboardInterrupt
```

### Make your own exceptions

```python
class PizzaError(Exception):
    def __init__(self, pizza, message):
        Exception.__init__(self, message)
        self.pizza = pizza

## you can raise your own exception as you would with any other
raise PizzaError("margherita", "Pizza is not on the menu")
```

## Assert

```python
i = int(input("Enter a number: "))

assert i != 0
print(1/i)
```

We assert that `i != 0`

- If this condition is true, nothing happends and code continues normally
- If this condition is false (user enters `0` in this case), we get an `AssertionError`

## Classes

```python
class Auto:
    something = 0  # class variable

    def __init__(self):
        self.name = "alfa romel"  # instance variable
        self._number = 123456  # private instance variable
        self.__another_number = 123456  # *name mandling* instance variable

auticko1 = Auto()
auticko2 = Auto()
```

- `class variable` is shared amongst all instances. If you share it in one instance, it will change everywhere
- `instance variable` is specific to one instance only. Each instance can have it different

- `private variable` should be only visible for corresponding class, and you should not access it although it can be accessed in come cases
- `name mandling variable` similar to private, but you have to specify the class name if you want to access it, like this: `print(auticko1._Auto.__another_number)`

### Some implicit attributes

Let's say we have simple class like this

```python
class Animal:
    something = 4

    def __init__(self):
        self.color = "blue"

    def method(self):
        pass

wolf = Animal()
```

```python
# __dict__
print(wolf.__dict__)  # {'color': 'blue'}
print(Animal.__dict__)  # {'__module__': '__main__', 'something': 4, '__init__': <function Animal.__init__ at 0x7e5df201d760>, 'method': <function Animal.method at 0x7e5df201cc20>, '__dict__': <attribute '__dict__' of 'Animal' objects>, '__weakref__': <attribute '__weakref__' of 'Animal' objects>, '__doc__': None}
```

```python
# __name__
print(type(wolf).__name__)  # Animal
print(Animal.__name__)  # Animal
```

```python
# __module__
print(wolf.__module__)  # __main__
print(Animal.__module__)  # __main__
```

```python
# __bases__
# shows parent classes
class SuperAnimal(animal):
    pass

print(Animal.__bases__)  # (<class 'object'>,)
print(SuperAnimal.__bases__)  # (<class '__main__.Animal'>,)
```

### Methods

`__str__()` method  
When you want to `print(instance_name)`, it usually returns something like `<__main__.Star object at 0x784e30330410>`  
You can change it with this method  
Something like overloading `>>` operator in C++

```python
class Star:
    def __init__(self, name, galaxy) -> None:
        self._name = name
        self._galaxy = galaxy

    def __str__(self):
        return self._name + " in " + self._galaxy + " galaxy"

sun = Star("Sun", "Milky Way")
print(sun)  # Sun in Milky Way galaxy
```

`__iter__()` and `__next__()`

```python
class Counter:
    def __init__(self, start, end):
        self.current = start
        self.end = end

    def __iter__(self):
        return self  # The iterator is the object itself

    def __next__(self):
        if self.current > self.end:
            raise StopIteration  # End of iteration
        value = self.current
        self.current += 1
        return value

# Usage
counter = Counter(1, 5)
for number in counter:
    print(number)  # Outputs 1, 2, 3, 4, 5
```

`issubclass(class_one, class_two)` returns `True` if `class_one` is child class of `class_two`  
`isinstance(object_name, class_name)` returns `True` if `object_name` is instace of `class_name`

## `yield`

The yield keyword in Python is used in generators, a type of iterable that generates items one at a time as needed, rather than creating all the items upfront and storing them in memory  
It's a key feature for writing efficient, memory-friendly iteration

1. Pauses Function Execution: When yield is encountered, the function's execution is paused, and the value after yield is returned to the caller.
2. Resumes Execution: The function can resume execution from where it left off when the generator's **next**() method is called again.
3. Creates a Generator: When a function contains yield, it automatically becomes a generator function, returning a generator object when called.

```python
def count_up_to(max_value):
    current = 1
    while current <= max_value:
        yield current  # Pause and return the current value
        current += 1

# Using the generator
counter = count_up_to(5)

# Consume the generator
for number in counter:
    print(number)  # Outputs: 1, 2, 3, 4, 5

# another example
def fun(n):
    for i in range(n):
        yield i


for v in fun(5):
    print(v)  # Outputs: 0, 1, 2, 3, 4
```

## Inline stuff

### Conditoins

```
print(True if 0 >= 0 else False)
```

### Generators

```python
the_list = [1 if x % 2 == 0 else 0 for x in range(10)]
```

### Lambda function

```python
lambda square x: x*x
print(square(5))  # 25
```

```python
lambda poly x: 2 * x**2 - 4 * x + 2
print(poly(16))
```

```python
def foo(x, f):
    return f(x)

print(foo(9, lambda x: x ** 0.5))
```

#### Lambda with `map()` and `filter()`

```python
short_list = ['mython', 'python', 'fell', 'on', 'the', 'floor']
new_list = list(map(lambda s: s.title(), short_list))

print(new_list)  # Mython, Python, Fell, ...
```

```python
short_list = [1, "Python", -1, "Monty"]
new_list = list(filter(lambda s: isinstance(s, str), short_list))

print(new_list)  # Python, Monty
```

## `os` module

```python
import os

print(os.uname())  # posix.uname_result(sysname='Linux', nodename='HP-Victus-Endeavour', release='6.12.1-arch1-1', version='#1 SMP PREEMPT_DYNAMIC Fri, 22 Nov 2024 16:04:27 +0000', machine='x86_64')
print(os.name)  # posix

os.mkdir("my_new_directory")
print(os.listdir())

os.mkdirs("my_new_directory/another_directory")
os.chdir("my_new_directory")
print(os.listdir())

os.rmdir("another_directory")
os.removedirs("my_new_directory/another_directory")

print(os.getcwd)  # pwd in linux
```

### `os.system()`

Let's you execute system commands

```python
os.system("ls")
os.system("pwd")
os.system("echo $SHELL")
os.system("fastfetch")
```

## Datetime

```python
from datetime import date

my_date = date(2020, 9, 29)
print("Year:", my_date.year) # Year: 2020
print("Month:", my_date.month) # Month: 9
print("Day:", my_date.day) # Day: 29
```

```python
from datetime import date
print("Today:", date.today()) # Displays: Today: 2020-09-29
```

```python
from datetime import date
import time
```

```python
from datetime import date
import time

timestamp = time.time()
d = date.fromtimestamp(timestamp)
```

```python
from datetime import time

t = time(13, 22, 20)

print("Hour:", t.hour) # Hour: 13
print("Minute:", t.minute) # Minute: 22
print("Second:", t.second) # Second: 20
```

```python
import time

time.sleep(10)
print("Hello world!") # This text will be displayed after 10 seconds.
```

```python
from datetime import datetime

dt = datetime(2020, 9, 29, 13, 51)
print("Datetime:", dt) # Displays: Datetime: 2020-09-29 13:51:00
```

```python
from datetime import date

d = date(2020, 9, 29)
print(d.strftime('%Y/%m/%d')) # Displays: 2020/09/29
```

%Y – returns the year with the century as a decimal number;
%m – returns the month as a zero-padded decimal number;
%d – returns the day as a zero-padded decimal number;
%H – returns the hour as a zero-padded decimal number;
%M – returns the minute as a zero-padded decimal number;
%S – returns the second as a zero-padded decimal number.

```python
from datetime import date

d1 = date(2020, 11, 4)
d2 = date(2019, 11, 4)

d = d1 - d2
print(d) # Displays: 366 days, 0:00:00.
print(d * 2) # Displays: 732 days, 0:00:00.
```
