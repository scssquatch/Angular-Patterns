footer: © Sharethrough, November XX 2015
slidenumbers: false

# Angular Patterns

---

## Conversion Overview

- Renaming files
- Converting rails templates to angular templates
- Front-end routing
- Front-end permissions
- Fetching data from Rails API
- Creating API controllers

---

## Let's start with renaming

`deals/deals_index_controller.js.coffee`
becomes
`deals/index.controller.js.coffee`[^1]

`deals/api_service.js.coffee`
becomes
`deals/data_service.js.coffee`


[^1]: [https://github.com/johnpapa/angular-styleguide#naming](https://github.com/johnpapa/angular-styleguide#naming)

---

## What that looked like for deals

![inline](rename\ before.png)


![inline](rename\ after.png)

^ as you can see, some were already correct, others not

---


# Nested Lists

- You can create nested lists
    1. by indenting
    1. each item with 
    1. 4 spaces
- It’s that simple

---

# Links

Create links to any external resource—like [a website](http://www.decksetapp.com)—by wrapping link text in square brackets, followed immediately by a set of regular parentheses containing the URL where you want the link to point:

`‘[a website](http://www.decksetapp.com)’`

Your links will be clickable in exported PDFs as well! 

---

# Display formulas

Easily include mathematical formulas by enclosing TeX commands in `$$` delimiters. Deckset uses [MathJax](http://www.mathjax.org/) to translate TeX commands into beautiful vector graphics.

Formula support is available as in-app purchase. See the next slide for an example.

<a name="formulas"></a>

---

## Schrödinger equation

The simplest way to write the time-independent Schrödinger equation is $$H\psi = E\psi$$, however, with the Hamiltonian operator expanded it becomes:

$$
-\frac{\hbar^2}{2m} \frac{d^2 \psi}{dx^2} + V\psi = E\psi
$$

---

# Captioned Images and Videos

![inline](room.jpg)

Easily create captions using [inline] images/videos with text underneath.

---

# Plus: 

- PDF export for printed handouts
- Speaker notes and rehearsal mode
- Switch theme and ratio on the fly
- Animated GIFs for cheap wins and LOLs :-)
