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
2) You can instantiate them without a value
2) Their declarations get hoisted
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

#### 2 "You can instantiate them without a value"



#### 3 "They get hoisted" 🏋

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

There's a dark side of hoisting, though. Javascript's compiler can be a bit *too* helpful at times.

If you use a variable within a function but don't instantiate that variable in that function, the compiler will look for a `var` in the parent function scope. It will keep looking all the way up the chain until it gets to the root scope of the entire application. If it still doesn't see a `var` declaration up there, it will just go ahead and make one for you, like so
```javascript
function logC() {
  c = 2;
  console.log(c); // 2
}
// c is not instantiated in this program, yet it just works...
```

I guess this is maybe convenient sometimes when writing tiny snippets or something...

But for the most part, I think this feature of Javascript hurts more than it helps. `"use strict";` prevents this behavior and should absolutely be included at the top of any ES5 script.

### How Times Have Changed

### Let me explain...

### Const you see the benefits?

### When & why?

### Use const until you have to use let


<!--
OUTLINE:
[] what we had before: var
[] what we have now: const and let
[] details of let
[] details of const
[] when to use const
[] when to use let
[] conclusion (use const until you have to use let, never use var)
-->

