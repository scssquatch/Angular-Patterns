autoscale: true
footer: © Sharethrough, November 11 2015
slidenumbers: false

# Angular Patterns

^ Thank you all for attending
- hoping we can do something like this pretty frequently

---

# Conversion Overview

- Renaming files
- Converting templates
- Fetching data from Rails API
- Creating API controllers
- Front-end routing
- Front-end permissions

^ Here are a few things we're gonna go over
- Some in more detail than others, you can always check out my PR for more detail
- or just ask me after this

---

# Renaming[^1]

`deals/deals_index_controller.js.coffee`
_becomes_
`deals/index.controller.js.coffee`

`deals/api_service.js.coffee`
_becomes_
`deals/data_service.js.coffee`

^ doesn't necessarily have to be index/show, etc.
- just name it after what it's for/ what it does


[^1]: [https://github.com/johnpapa/angular-styleguide#naming](https://github.com/johnpapa/angular-styleguide#naming)

---

# Renaming

![inline](rename\ before.png)


![inline](rename\ after.png)

^ as you can see, some were already correct, others not
- data_service vs api.data_service?
- now lets go over converting the actual templates (next)

---

# Template Conversion

![inline](haml\ before.png)

^ Notice a few things
- ugly bracket syntax
- can can usage in markup
- in-line rails for model data (mix with angular too)
- javascript block for bootstrapping data
- render partial syntax
- random double quotes
- hrefs
- and here's where I ended up (next)

---

# Template Conversion

![inline](haml\ after.png)

^ more readable attributes using parentheses
- all single quotes unless otherwise needed
- no ng-controller declaration (will address later)
- ui-sref instead of href
- if-can directive
- ng-bind over mustaches
- all rails data lives on controller
- ng-include over render partial (will go over more)
- no more :javascript
- here's how we get data instead (next)

---

# Getting Rails Data

![inline](Deals\ Index\ Controller.png)

^ inject deals data service
- make call, resolve promise and set to instance variable
- what if you need deal before page loads? (We'll get to that later)
- and heres the data service (next)

---

# Getting Rails Data

![inline](Deals\ Data\ Service.png)

^ super simple data service
- could live in bakery.shared, but for now leave here (due to future changes)
- call api endpoint with url with key
- and heres the api controller (next)

---

# Getting Rails Data

![inline](Deals\ Api\ Controller.png)

^ very simple
- includes is for rendering seat name without n+1 queries
- and here are some of those routes that we needed to create
- authorize resource for ensuring user can see deal

---

# Getting Rails Data

![inline](routes-2.png)
![inline](routes-1.png)

^ Nothing too crazy here
- api internal controller
- globbing for angular routing
- Home controller index action is blank
- Now lets move on to partials (next)

---

# Angular Partials

`deals/index.nghaml`

![inline](ng-include.png)

`deals/bar_table.nghaml`

![inline](ng-include-2.png)

^ specify the source (directory, name)
- set internal controller if it is shared in onLoad
- partial now has access to dealsCtrl through metricsCtrl
- lets talk about maybe using ui-view in the future?

---

# Angular Partials

In case wrapping the partial causes issues

![inline](ng-include-replace.png)

^ This is for things that have css like div > .foo
- you can use this directive everywhere else too

---

# Angular Routing[^2]

![inline](deals-router-1.png)

[^2]: [https://github.com/angular-ui/ui-router](https://github.com/angular-ui/ui-router)

^ - remember when I got rid of ng-controller? It's here
- lets focus on deals state first
- url, template, controller, as
- parent
- parent state
- ui-view template
- data permissions (will go over this in a minute)
- how do i link to these states? (next)

---

# Angular Routing

New and Show routes:

![inline](new-vs-show.png)

^ two different state routes
- one with no params, the other with a required dealkey param
- both Will trigger state changes, change URL, not reload browser
- you can also pass arbitrary params to routes and handle them on a case-by-case basis
- how about transitioning states in a controller? (next)

---

# Angular Routing

<br>

![inline](controller-route-go.png)

^ inject state service and call state.go with the state and optional params
- now lets go over permissions (next)

---

# Route Authorization

`deals/router.js.coffee`

![inline](route-permissions.png)

`shared/permissions.constant.js.coffee`

![inline](permissions-constant.png)

^ So, remember that data-permission hash?
- Make sure you have all permissions in the constant file
- so lets say we wanted to change readDeals in the router to manageDeals (next)

---

# Route Authorization

![inline](missing-permission.png)

^ Auth will fail, and you'll get this console error message
- Now lets say we added manageDeals to the permissions constant, that's it right? Wrong (next)

---

# Route Authorization

![inline](missing-permission-service.png)

^ Auth will still fail, and you'll get this console error message
- Now we add manageDeals to the permissions service here (next)

---

# Route Authorization

<br>

![inline](custom-permission-check.png)

^ This is part of a switch statement in the main canAccess method
- See here you can use a 'can' method, or do something custom
- you can modify this file and allow custom attributes or something other than key potentially
- checking role or pub key to only allow user to see their own pub content
- you can also do custom checking like this

---

# Route Authorization

<br>

![inline](permissions-can.png)

^ Heres the basic method that does 90% of the work
- There's a lot of technical details that you can look up if you're interested
- checks against manage first, since that encompasses all actions of a feature/resource
- maybe feature is the wrong variable name here
- but basically it ends up here, checking against the users cancan abilities
- and here is where the permission service gets called during routing (next)

---

# Route Authorization

![inline](route-auth.png)

^ lots of stuff here again, but it's not super important to understand
- check if logged in, unless state is login (doesn't exist yet)
- should probably just return if state is login
- basically the parent publisher stuff might be important
- if there is a parent you must be able to see parent
- make sure you can also access curent
- if unauthorized, go to a pseudo 404 page
- prevent default if you are not logged in
- get current user (pseudo log in)
- set user in session, recurse
- okay cool, now how about using auth/permissions on the front-end? (next)

---

# ifCan Directive

The frankenstein if-else

![inline](ui-sref.png)

^ would love to hear if someone knows a better way to do this
- One last thing
- what if you need data that is required before the page loads?
- the page jitters and snaps after resource is loaded. Here's a solution (next)

---

# Router Resolving

![inline](pre-resolve.png)

^ Router resolving is a very cool concept
- It takes code from this (next)

---

# Router Resolving

![inline](post-resolve.png)

^ ...to this
- now where does this magical deal object coming from? (next)

---

# Router Resolving

![inline](route-resolve.png)

^ here!
- Use route resolves for data that is required before the page loads
- will cause initial page load to take longer so...
- we need to establish a pattern showing that the page is loading
- but it also gets rid of lots of prepareData calls and jittery pages
- Now here are a few gotchas I came across while doing this (next)

---

# Gotchas and Interestings

Coffeescript + Inline-Edit

![inline](inline-edit-method.png)
![inline](inline-edit-method-2.png)

^ returning promise made the http request twice

---

# Gotchas and Interestings

Boostrap Modals

![inline](modal-resolve.png)

^ result callback when modal.close called
- resolve data into it, without using extra unneeded service
- added benefit of it being obvious that the modal needs these values (breaks if not present)

---

# Gotchas and Interestings

Boostrap Modals

![inline](modal-controller.png)

^ placements and dealKey *must* be resolved into it, even if it is just null

---

# Gotchas and Interestings

Camelizing and Decamelizing $http requests

![inline](camelize-http.png)

^ This is something I'd like to implement sometime soon
- will require another large branch probably :\

---

# Further Discussion

- Coffeescript: CS classes vs VM syntax?
- str-directive-name?
- Plan for separating front-end from back-end
- Better handling duplication like metrics-related stuff
- relying on cancan abilities JSON
- plan for future refactorings?
- feature flagged angular routing?

---

# References

[John Papa's Styleguide](https://github.com/johnpapa/angular-styleguide)

[AngularUI Router](https://github.com/angular-ui/ui-router)

[Angular Devise](https://github.com/cloudspace/angular_devise)

[Pull Request](https://github.com/sharethrough/bakery/pull/1018)

---

![inline](thanks.gif)

