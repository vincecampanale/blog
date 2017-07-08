---
layout: post
title:  "Use const until you have to use let"
description: "The differences between Javascript (ES6) variable instantiation keywords -- const and let -- and when to use them."
date: 2017-06-22
---

Aloha, web surfer. So perhaps you're a `let` fan who came here to see what was up. Or maybe you're a beginner, just moving out of `var` town and into the wild west that is ES6 variable declaration. Whatever your experience level may be, I hope this article provides valuable insights and leaves you with a solid rule of thumb for instantiating variables in Javascript. <!--Also, I think it's worth mentioning that throughout this article I will be using "Javascript" in lieu of the more specific "ES6" because in June 2017, I'd like to believe that the language features introduced in ES6 can now be thought of as just Javascript rather than some other language.--> My other hope is that, upon finishing this article, you can confidently part ways with `var` -- it is time.

### Once Upon A Time...
...in a specification authored somewhere in California...

...a keyword for variable instantiation was created and called `var`. 

If you remember your first day of learning Javascript, it probably involved `var`. This little keyword, short for `variable`, took us a long way. In fact, every web site and application that used Javascript between it's inception in 1995 and the release of ECMAScript 2015 (ES6) used `var` and only `var` to create variables. 

The `var` keyword is no longer relevant, but in order to truly understand our present, we must understand our past.

Some important things to know about variables created with the `var` keyword: 
```
1) They are function scoped
2) Their declarations get hoisted
3) You can instantiate them without a value
```

#### 1 "They are function scoped"

This means that `var` creates a variable that only exists inside of the function where it was declared. 

*(Unless it was not declared inside a function. In that case, it exists ~everywhere~ 😲. More on this in fact #3.)*

Here's a code snippet that illustrates the behavior of a function scoped variable: 
```javascript
function logA() {
  var a = 2;
  console.log(a); // inside of `logA()`, `a` exists with value 2
}

try { console.log(a); } // doesn't work...error goes to catch block...
catch (error) { console.log(error); } // ReferenceError! This means that outside of logA(), `a` is not defined.
```

I think it's worth clarifying that when I say "inside of" `logA()`, I mean "between `logA()`'s curly brackets".

#### 2 "They get hoisted" 🏋

Variable hoisting basically boils down to the fact that a variable declared with `var` can be instantiated *after* it gets used. 

This code...
```javascript
function logB() {
  b = 2; // using b to hold the number 2
  console.log(b); // using b to print 2 in the console

  var b; // instantiating b 🤔
}
```

...will get transformed into something like this code...

```javascript
function logB() {
  var b; // moved to top of function scope

  b = 2;
  console.log(b);
}
```
...at *compile* time (yes, [Javascript gets compiled](https://github.com/getify/You-Dont-Know-JS/blob/31e1d4ff600d88cc2ce243903ab8a3a9d15cce15/scope%20%26%20closures/ch1.md)).

There's a dark side to the way `var` gets hoisted, though. Javascript's compiler can be a bit *too* helpful in some situations.

If you use a variable within a function but don't instantiate the variable in that function, the compiler will look for a `var` in the parent function scope. If it doesn't find it there, it will keep looking all the way up the scope chain until it gets to the *global* scope. If it *still* doesn't see a `var` declaration up there, it will just go ahead and *make one for you*, as demonstrated in this snippet:
```javascript
function logC() {
  c = 2;
  console.log(c); // 2
}
// c is not instantiated in this program, yet it just works...
```
Yikes.  

I guess this could be convenient sometimes when messing around in a REPL or something...

But for the most part, I think this is bananas. `"use strict";` prevents this and should absolutely be included at the top of any ES5 script.

#### 3 "You can instantiate them without a value"

You may have seen someone do something along these lines before:
```javascript
// declare all my variables at the top of the script
var a, b, c;

// use them down here
a = 1;
b = 2;
c = a + b;
```

Put a pin in this because it becomes important later. 

### Times Have Changed

The three facets of `var` that I just covered come together to create an extremely flexible variable declaration mechanism. While flexibility itself is not a bad thing and having only one variable instantiation keyword means a little more flexibility is necessary, `var` is simply doing too much.  Function scope isn't a problem, but reassigning variables is so-so and should be used *only when completely unavoidable* (and when it's unavoidable, it means you probably have bigger fish to fry). The same goes for declaring variables without values. Lastly, being allowed to use a variable whether you declared it or not is just silly.

So, in order to address these drawbacks of `var` and save us developers from the pitfalls these "helpful" features can cause, the ES6 specification introduced two new variable keywords: `const` and `let`.

As I said in the first paragraph, my primary goal of this post is to help you ditch `var`. Times have changed and `var` doesn't make the cut.

### Let me explain...

In 2015, Javascript's variable declaration family grew to include `let`, which deviates significantly from `var` in that it is *block* scoped. This basically means that we now have two units of scope available:  

#### 1 Functions
```javascript
function() {
  let i = 'aye'; // `i` is scoped to this function
  console.log(i); // "aye"
}
console.log(i); // ReferenceError
```
#### 2 Blocks
```javascript 
{
  let i = 'oi'; // `i` is scoped to this block
  console.log(i); // "ai"
}
console.log(i); // ReferenceError
```

The `let` keyword has another helpful constraint: the variable cannot be accessed *before* its declaration (`let` [does get hoisted](https://stackoverflow.com/questions/31219420/are-variables-declared-with-let-or-const-not-hoisted-in-es6) though). 

One thing `let` shares with `var` is that it can be used to declare a variable without an initial value.
Like in the example above, we could have done
```javascript
{
  let i;
  i = 'oi';
}
```
and it would've worked.

#### Bookmarks for `let` 👓
* [Are Let / Const Hoisted?](https://stackoverflow.com/questions/31219420/are-variables-declared-with-let-or-const-not-hoisted-in-es6)
* [YDKJS Blocks As Scopes](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch3.md#blocks-as-scopes)
* [YDKJS Let](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch3.md#let)
* [YDKJS Garbage Collection](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch3.md#garbage-collection)
* [YDKJS Let loops](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch3.md#let-loops)


### Const stop, won't stop

Okay, `const` is my favorite variable declaration keyword in Javascript. I default to `const` over `let` and never use `var`. If you remember one thing from this post, let it be this: Use `const` until you have to use `let`. A [wise man](https://medium.com/javascript-scene/javascript-es6-var-let-or-const-ba58b8dcde75) told me that.

In the sub-section entitled **3 "You can instantiate them without a value"** in the `var` portion of the article, I said to put a pin in the fact that variables can be declared using `var` with empty values. This type of instantiation is allowed with `let`, too.  It is not so with `const`! Oh no, `const` requires that you provide a value up front. Otherwise, it will turn its nose up at you and refuse to create a variable, *"If you don't know what's going *in* the variable, monsieur/madame, then why do you need it at all?"* -- this is good. 

But wait, there's more! `const` goes one step further and ensures that the variable cannot be reassigned.

```javascript
```

### When & why?

### Use const until you have to use let

<!--
OUTLINE:
[x] what we had before: var
[x] details of let
[] details of const
[] when to use const
[] when to use let
[] conclusion (use const until you have to use let, never use var)
-->

