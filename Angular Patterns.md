autoscale: true
footer: © Sharethrough, November XX 2015
slidenumbers: false

# Angular Patterns

---

# Conversion Overview

- Renaming files
- Converting templates
- Front-end routing
- Front-end permissions
- Fetching data from Rails API
- Creating API controllers

---

# Renaming[^1]

`deals/deals_index_controller.js.coffee`
_becomes_
`deals/index.controller.js.coffee`

`deals/api_service.js.coffee`
_becomes_
`deals/data_service.js.coffee`


[^1]: [https://github.com/johnpapa/angular-styleguide#naming](https://github.com/johnpapa/angular-styleguide#naming)

---

# Deals Example

![inline](rename\ before.png)


![inline](rename\ after.png)

^ as you can see, some were already correct, others not

---

# Rails Template

![inline](haml\ before.png)

^ Notice a few things
- ugly bracket syntax
- can can usage in markup
- hrefs
- in-line rails for model data
- javascript block for bootstrapping data
- render partial syntax
- random double quotes

---

# Angular Template

![inline](haml\ after.png)

^ Good changes
- more readable attributes
- no ng-controller declaration
- if-can directive
- all rails data lives on controller
- ng-include over render partial
- all single quotes unless otherwise needed
- ui-sref instead of href

---

# Getting Rails Data

![inline](Deals\ Data\ Service.png)

^ super simple data service
- could live in bakery.shared, but for now leave here
- call api endpoint with url with key

---

# Getting Rails Data

![inline](Deals\ Index\ Controller.png)

^ inject deals data service
- make call, resolve promise and set to instance variable
- what if you need deal before page loads? We'll get to that later

---

# ifCan Directive

If can:

##`%a(if-can='manageDeals' ui-sref='dealsNew') New Deal`

If cannot:

##`%span(if-can='!manageDeals') Cannot Text`

^ would love to hear if someone knows a better way to do this

---

# Angular Partials

In deals/index.nghaml:

![inline](ng-include.png)

In deals/bar_table.nghaml:

![inline](ng-include-2.png)

^specify the source (directory, name)
- set internal controller if it is shared in onLoad
- partial now has access to dealsCtrl/metricsCtrl

---

# Angular Partials

In case wrapping the partial causes issues:

![inline](ng-include-replace.png)

^ This is for things that have css like div > .foo
- you can use this directive everywhere else too

---

# Angular Routing[^2]

![inline](deals-router-1.png)

[^2]: [https://github.com/angular-ui/ui-router](https://github.com/angular-ui/ui-router)

^ lots going on here
- lets focus on deals state first
- url, template, controller, as
- parent
- parent state
- ui-view template
- data permissions

---

# Angular Routing

<br>

##`%a(ui-sref='dealsNew') New Deal`

^ Will trigger state change

---

# Angular Routing

<br>

##`%a(ui-sref="dealsShow({ dealKey: deal.key})") Show Deal`

^ You can send any params through there

---

# Angular Routing

![inline](route-permissions.png)
![inline](permissions-constant.png)

^ Make sure you have all permissions in the constant file
- if you don't, don't worry, you'll be reminded

---

# Angular Routing

![inline](missing-permission.png)

---

# Angular Routing

<br>

![inline](permissions-can.png)

^ There's a lot of technical mumbo jumbo that you can read if you have time
- but basically it ends up here, checking against the users cancan abilities
- you can also do custom checking like this

---

# Angular Routing

<br>

![inline](custom-permission-check.png)

^ See here you can use that can method, or do something custom
- you can modify this file and allow custom attributes or something like that
- checking role or pub key to only allow user to see their own pub content

---

# Route Authorization

![inline](route-auth.png)

^ lots of stuff here again, but it's not super important to understand
- basically the parent publisher stuff might be important
- logged in, unless state is login (doesn't exist yet)
- if there is a parent you must be able to see parent
- make sure you can also access curent
- if unauthorized, go to a pseudo 404 page
- prevent default if you are not logged in
- get current user (pseudo log in)
- set user in session, recurse

---

# State Changes

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
