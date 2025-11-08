# CSS
Cascading Style Sheets  
Used to style websites (HTML files)   
> Note: CSS is not and programming language

One day I will also discuss [Tailwind CSS](./Tailwind.md)

# Basic stuff
There are 3 ways to style and element  

## Inline Styles
```html
<p style="color: red">Some red text</p>
```
Your code can get a bit messy if you use these too much  
You have to write the style on every element you want to apply the style to (not practical if you have a lot of elements)

## `<style>` tag
```html
<style>
	p {color: red}
	h2 {color: blue}
</style>
```
Every `<p>` tag will have red color and every `<h2>` tag will have blue color  
The style should be located at the start of `<body>` tag, so the browser knows about all the styles before rendering elements  

## `.css` file
File like `styles.css`  
```css
p {
	color: red;
}

h2 {
	color: blue;
}
```

CSS files needs to be imported to HTML file in order to work  
```html
<head>
	<link rel="stylesheet" href="styles.css">
</head>
```

Now every `<p>` tag will have red color and every `<h2>` tag will have blue color  
I would say that this method is mostly used   
You can make one `.css` file and import it to multiple `.html` files, so they will be styled the same without writing the code multiple times  

## Comments in CSS
The comment starts with `/*` and ends with `*/`  
You can use this for both single-line and multi-line comments  
```css
/* Example of single-line comment */
```

```css
/* Example
of 
multi-line
comment */
```

# Colors
I already stated something of this in HTML, there is something more to it  

## Color names
So far, we specified color with it's name, like `red` or `blue`  
It's good for the start, but it has a major problem - the number of colors is limited  
Maybe you will want a really specific color one day, and it will not be in [the list of named colors](https://www.w3schools.com/colors/colors_names.asp)  
There is a really good solution for this: **HEX** *(hexadecimal)* and **RGB** *(red, green, blue)* colors  

| Color name | Hex color code | RGB color code   |
| ---------- | -------------- | ---------------- |
| Red        | `#ff0000`      | `rgb(255, 0, 0)` |
| Green      | `#00ff00`      | `rgb(0, 255, 0)` |
| Blue       | `#0000ff`      | `rgb(0, 0, 255)` |
## HEX
`#ff0000`  
In **HEX** you are using 6 hexadecimal characters - `0` to `9` and `a` to `f`  
You can consider `a` to be `10`, `b` to be `11` and all the way to `f` being `15`  
It's organized in three pairs: First two characters defines `red`, second two defines `green`, third two defines `blue`  
`0` means that the color is not present at all, while `f` means that the color is fully present  
> See `.example2` in [conclusion](#Colors%20conclusion)

It's generally recommended and considered a better practice to use HEX colors instead of Color names  

### HEX shorthand
`#f00`
As mentioned before, HEX colors are organized in 3 pairs  
If each pair has both numbers the same, you can only use only one of these numbers  
It might not make a sense in sentence like this, so there is and example  
> See `.example3` in [conclusion](#Colors%20conclusion)

| Full HEX value | Simplified HEX value |
| -------------- | -------------------- |
| `#ff0000`      | `#f00`               |
| `#112233`      | `#123`               |
| `#aa3300`      | `#a30`               |
| `#000000`      | `#000`               |

It's just a way to save you some time on typing  
Just remember that *all* 3 pairs have to have both numbers the same  

> Note: It doesn't matter if you use uppercase or lowercase letters in HEX values, just stick to one and use it everywhere  

## RGB
`rgb(255, 0, 0)`
In **RGB** you are using 3 numbers separated by comma (`,`), which ranges from `0` to `255`  
As the name states, first number specifies `red`, second one specifies `green`, third one specifies `blue`  
Similarly to HEX, `0` means that the color is not present at all, while `255` means that the color is fully present  
> See `.example4` in [conclusion](#Colors%20conclusion)

It's better to use RGB than Color names, but I think HEX is used more than RGB in web development  

### RGBA
`rgba(255, 0, 0, 1)`
It is something like an extension to RGB  
The `A` in `RGBA` stands for alpha, which basically defines opacity/transparency of an element  
It can be value from `0` to `1`, where `0` is fully transparent element and `1` is fully opaque element  
It's nice when you want to make like a half-transparent element, which could be created like this: `rgba(255, 255, 255, 0.5)`  
> See `.example5` in [conclusion](#Colors%20conclusion)

## Colors conclusion
Example on all methods to write colors  
```css
/* color defined with it's name */
.example1 {
	color: red;
}

/* color defined with hex */
.example2 {
	color: #ff0000;
}

/* color defined with hex with shortcut */
.example3 {
	color: #f00;
}

/* color defined with rgb */
.example4 {
	color: rgb(255, 0, 0);
}

/* color defined with rgba */
.example5 {
	color: rgba(255, 0, 0, 1); 
}

```
All of these classes will make the exact same result  

You don't have to struggle with these, modern text editors (like VS Code or WebStorm) have their solution to display these colors via plugins  
You might also use one of many free online tools to convert between these colors, like the one from [w3schools](https://www.w3schools.com/colors/colors_converter.asp)

# Classes and ID's
## Class
If you want to style multiple elements the same, you can use something called *class*
```css
.blue-text {
	color: blue;
}
```

```html
<div>
	<h1>Welcome</h1>
	<h3 class="blue-text">How have you beed?</h3>
	<p class="blue-text">Hello there</p>
</div>
```

In this case `blue-text` is a class  
You can add multiple properties to `.blue-text` inside `.css` file and apply it to different elements in HTML  

You can use multiple classes on one element  
They are separated with just whitespace   
```css
.green-text {
	color: green;
}

.big-text {
	font-size: 32px;
}
```

```html
<p class="green-text big-text">Hello</p>
```
Now you have made a big green text :)


> Notice how the class is defined vs. used  
> To define a class in `.css`, you add a dot (`.`) before the class name  
> To use the class, you just type it's name without the dot  

## ID
ID's work similarly to classes, with one main difference - only one element can have one ID, while multiple elements can have one Class  
```css
#main-title {
	color: yellow;
}
```

```html
<h1 id="main-title">Cypo's website</h1>
```

You can't use the ID `title` on another element, because it's already used somewhere  
If you used the ID on another element, neither of them will work  

## Class vs. ID
You can use both Class and ID on one element  
ID has higher priority over Class  
```css
.random-class {
	color: blue;
}
#random-id {
	color: red;
}
```

```html
<p class="random-class" id="random-id">Hello</p>
```
This text will be red, because ID has higher priority over Class

| Class                     | ID                               |
| ------------------------- | -------------------------------- |
| Used on multiple elements | Used only on one element         |
| Defined with `.` in CSS   | Defined with `#` in CSS          |
| More used                 | Less used                        |
| Lower priority\*          | Higher priority\*                |
|                           | Easier to access with JavaScript |

\*More about priorities in [Priorities section](#Priorities)

---

Now let's have a look on few more CSS attributes, because we only used `color` so far   

# `font-size`
```css
h1 {
	font-size: 32px;
}
.text {
	font-size: 8px;
	color: red
}
```
Now every `<h1>` tag will be 32 pixels high and every element with `text` class applied will be 8 pixels high and red in color   

# `font-family`
```css
.text {
	font-family: monospace;
}
```
All elements with `text` class applied will have `monospace` font  
`monospace` is one of the basic fonts, that is usually available on all browsers  
However, if you wanted to use less usual font or whatever, you can do it by importing font from [Google fonts](https://fonts.google.com) for example  

## Import a Google font
You will simply go to [Google fonts](https://fonts.google.com) website and choose your desired font  
You can choose between various font weights and styles  
You can click on *Get embed code* and choose either `<link>` to import into HTML or `@import` to import it into CSS  

I prefer importing it in HTML with `<link>`  
It's as simple as copying provided code and pasting it inside `<head>` tag in your HTML  
> Note: if you want to import it inside CSS, you can paste it at the beginning of CSS file, so it will be available everywhere in the file  

It could look something like this  
```html
<head>
	<link href="https://fonts.googleapis.com/css2?family=Lobster&display=swap" rel="stylesheet">
	...
</head>
```

```css
p {
	font-family: Lobster, monospace;
}
```
Here I imported `Lobster` font and used it on all paragraphs  

> Notice how I put `monospace` after `Lobster` 
> This is called "backup font"
> This basically means, that `monospace` will be used, if `Lobster` is not available  

# Absolute vs. Relative units  
Before moving next, let's talk a bit about size units  
If you are typing size of some element, like a size of a font, you can either use and Absolute or Relative units
I can give you example on some of them   

Absolute has a fixed length value  
It doesn't matter if anything else changes, it will remain the same  

Relative is "relative" to another value  
If the other value changes, this one will change as well  

| Absolute units examples | Relative units examples             |
| ----------------------- | ----------------------------------- |
| `px` - pixels           | `%` - % of parent element           |
| `mm` - millimeters      | `em` - x times size of current font |
| `cm` - centimeters      | `vw` - viewport width               |
| `in` - inches           | `vh` - viewport height              |

If you had text with 12 pixels height, it will be 12 pixels high all the time  
Same with distance units, for example 4 millimeters will be 4 millimeters  

If you had a text height of `50vh`, it means that it will take up half of the screen height  
Simply resizing the window will change the size of a text  
If you had `<div>` with `25%` width, it means that it will take up $1/4$ of parent element  
If the parent element changes, the `<div>` will change as well  
If you had a `<p>` tag with size of `0.5em`, it will be half the size of normal `<p>`  

This might make more sense the more you work with these things *(as everything)*, but it's good to understand  

# Borders
```css
.thick-green-border {
	border-color: green;
	border-width: 10px;
	border-style: solid;
}
```

Previous code can be simplified to following...
```css
.thick-green-boder {
	border: 10px solid green;
}
```

## Rounded corners
```css
.thick-green-boder {
	border: 10px solid green;
	border-radius: 8px;
}
```
> Note: Don't forget to apply these classes to make the changes visible

# Images  
## Image size
```css
.normal-image {
	width: 200px;
	height: auto;
}

.large-image {
	width: 500px;
	height: auto;
}
```
`auto` makes it's height to adjust automatically, based on original aspect ratio  

## Rounded Image with `border-radius`
```css
.image {
	width: 33vw;
	border-radius: 50%;
}
```
> Note: example of using a [relative unit](#Absolute%20vs.%20Relative%20units) - this image will take up $1/3$ of whole screen horizontally  

# Background
```css
.silver-background {
	background-color: silver;
}
```

# Padding & Margin
I think the best way to explain Padding vs. Margin is with the following image  
![](css-padding-vs-margin.jpg)

I will try do describe it with words...  
- **Content** is the element you are styling, whenever it's a text, image, `div` or anything else  
- **Border** is like a *line around Content*  
- **Padding** is a space between *Content* and *Border* 
- **Margin** is a space *outside a border*. It's meant to separate different elements  

By default, the width of *Border*, *Padding* and *Margin* is set to zero, so you can't see them  

## Applying Padding/Margin/Border size
There is one hard way to apply Padding/Margin/Border, and there are few shortcuts  
```css
.hard-way {
	padding-top: 10px;
	padding-left: 10px;
	padding-bottom: 10px;
	padding-right: 10px;
}

.easier-way {
	padding: 10px; 
}
```
Both of these classes will produce the same result  
> Note: I will use only padding size from now, but it works the same for both `padding` and `margin` and even `border-width` 
### Clockwise notation  
You can use something called "Clockwise notation" to specify size of Padding/Margin/Border  
```css
.hard-way {
	padding-top: 10px;
	padding-left: 20px;
	padding-bottom: 30px;
	padding-right: 40px;
}

.easier-way {
	padding: 10px 20px 30px 40px;
}
```
Both of these classes will produce the same result  
It's called clockwise notation, because it works like a clock: starting with "top", then going clockwise - "right", "bottom", "left"  

### More shortcuts
#### Top-Bottom & Right-Left
```css
.hard-way {
	padding-top: 10px;
	padding-left: 30px;
	padding-bottom: 10px;
	padding-right: 30px;
}

.easier-way {
	padding: 10px 30px
}
```
Both of these classes will produce the same result  
If you provide 2 values: The first one will be for both Top and Bottom, the second one will be for both Left and Right  

#### Top, Bottom & Right-Left
```css
.hard-way {
	padding-top: 10px;
	padding-left: 20px;
	padding-bottom: 40px;
	padding-right: 20px;
}

.easier-way {
	padding: 10px 20px 40px
}
```
Both of these classes will produce the same result  
If you provide 3 values: The first one will be for Top, second one will be for both Left and Right, the third one will be for Bottom  

### Padding/Margin/Border shorthand summary

| Format             | Meaning                                                |
| ------------------ | ------------------------------------------------------ |
| `padding: a b c d` | Top = `a`<br>Right = `b`<br>Bottom = `c`<br>Left = `d` |
| `padding: a b c`   | Top = `a`<br>Right = Left = `b`<br>Bottom = `c`        |
| `padding: a b`     | Top = Bottom = `a`<br>Right = Left = `b`               |
| `padding: a`       | Top = Right = Bottom = Left = `a`                      |
# Attribute selectors to style element
```css
[type="radio"] {
	margin: 20px
}
```
Now you have applied a margin of 20 pixels to every radio button  

# Priorities
Imagine you have some CSS...  
One style is telling a `<p>` tag to be blue, another style is telling it to be red  
```css
p {
	color: blue;
}

.red-text {
	color: red;
}
```

```html
<p class="red-text">Hello</p>
```
So what would be the final color of the text?  
It will be red, because Class has higher *priority* than Element selector  

Priorities are defined with this list
1. `!important`
	- If you has CSS property like in example bellow, it has the highest priority among everything
	- `.box { padding: 20px !important; }`
1. Inline CSS
	- Specifying CSS inside HTML, in some element 
	- `<p style="color: blue">`
1. ID
	- If you have an ID in CSS (declared with `#`)
	- `#box { width: 50px }`
1. Class
	- If you have a Class in CSS (declared with `.`)
	- `.box { width: 50px }`
1. Element selector
	- If you wanted to apply a CSS to all elements of the same type 
	- `h2 { font-size: 2em }`

> Note: There is a bit more to priorities  
> You can follow this list to make it simpler  

# CSS variables  
You can use something like variables in CSS, similarly to some programming language  

```css
:root {
    --background: #222;
}
```

With this you have defined a variable for background color (you can choose whatever name you want)  
You can use this by doing following  

```css
.body {
	background-color: var(--background);
}
```

## CSS variable fallback
Variables might not be supported in all browsers, mainly in older ones  
In this case you can add a "fallback value" which will be used instead   

```css
.body {
	background-color: var(--background, gray);
}
```

# `hover`
With `:hover` you can control how element is styled when a mouse pointer is over it  

```css
div:hover {
	background-color: red;
}
```

There are a lot more [selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors) like this, for example `:active`, `:nth-child()`, `:focus` just to mention some  

# `position`
```css
.example1 {
	position: relative;
}

.example2 {
	position: absolute;
}

.example3 {
	position: fixed;
}

.example4 {
	position: sticky;
}
```

# CSS Flexbox  
Flexbox is a tool to organize elements in one dimension  
There is a lot to know about Flexbox, so I will add a [Flexbox cheatsheet](css-flexbox-cheatsheet.png)
![](css-flexbox-cheatsheet.png)

# CSS Grid
Grid is a tool to organize elements in two dimensions
There is a lot to know about Grid, so I will add a [Grid cheatsheet](css-grid-cheatsheet.png)
![](css-grid-cheatsheet.png)

# `animation`
```css
.animation {
    animation-name: colorful;
    animation-duration: 3s;
}

@keyframes colorful {
    0% {
        background-color: blue;
    }
    100% {
        background-color: yellow;
    }
}
```

```css
img:hover {
    animation-name: width;
    animation-duration: 500ms;
}

@keyframes width {
    100% {
	    width: 40px;
    }
}
```

# `z-index`
With `z-index` in CSS, you can specify which element will be on top of another, if two elements are covering each other  

```css
.on-top {
	z-index: 2;
}

.on-bottom {
	z-index: 1;
}
```

The higher the `z-index` is, the higher it will be displayed  
Just remember that the `z-index` is also based on it's parent `z-index`  


# `transform`
```css
.box {
	transform: scale(1.5);
	transform: matrix(1, 2, 3, 4, 5, 6);
	transform: translate(120px, 50%);
	transform: rotate(0.5turn);
	transform: skew(30deg, 20deg);
	transform: scale(0.5) translate(-100%, -100%);
}
```

Read more on these on [CSS transform by Mozilla](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)

# Gradients  
Most basic one is `linear-gradient`
When defining `linear-gradient`, you have to provide there information: `direction`, `color1`, `color2`, `color3`,...  

```css
.box {
	background: linear-gradient(35deg, #cff, #fcc)
}

.another-box {
	background: linear-gradient(to right, #abc, #123, #def, #456)
}
```

You can also provide percentage after each color, if you don't want them to be distributed equally  

```css
.different-box {
	background: linear-gradient(to top left, #333, #333 50%, #eee 75%, #333 75%);
}
```

Or you can specify two numbers 
```css
.totally-different-box {
	background: linear-gradient(
	    to right,
	    red 20%,
	    orange 20% 40%,
	    yellow 40% 60%,
	    green 60% 80%,
	    blue 80%
	);
}
```

## Other gradients 
- `repeating-linear-gradient`
- `radial-gradient`
- `conic-gradient`

Read more on these on [Using CSS gradients by Mozilla](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_images/Using_CSS_gradients#using_repeating_gradients)

# `@media` query
`@media` query is a way to style elements differently based on screen resolution of current device  
```css
@media (max-width: 100px) {
	.box {
		background-color: red;
	}
}

@media (max-height: 350px) {
	.box {
		background-color: blue;
	}
}
```
