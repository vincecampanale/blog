---
layout: post
title:  "What is Javascript's `new` keyword doing under the hood?"
description: ""
date: 2017-06-07
---

Good morning, afternoon, evening, or night. I have some things to share with you about the `new` keyword in Javascript. Important things. 

I'll start with some context and background about Constructor functions and the `class` keyword. Then, I will explain exactly what the `new` keyword is *doing* under the hood. Next,
 I will show *how* it is doing what it does by implementing it in code. Finally, I will explain *why* it does these things and give a couple arguments for avoiding this approach altogether in most situations. The information presented here comes from [these](https://www.youtube.com/watch?v=Y3zzCY62NYc) [resources](https://medium.com/javascript-scene/javascript-factory-functions-vs-constructor-functions-vs-classes-2f22ceddf33e), processed by my brain. 

### Constructor functions
A Constructor function is a function that builds and returns a new instance of object. It looks like this: 

```javascript
/** Car: {
*    doors: number,
*    color: string,
*    drive: Function
*   }
*
* CarConstructor(doors: number, color: string) => Car
*/

function CarConstructor(doors=4, color='red') {
    this.doors = doors;
    this.color = color;
    this.drive = () => console.log('Vroom!');
}
```

The capital letter at the beginning of the Constructor name is simply a convention adopted by Javascript programmers to separate *Constructor* functions from regular functions. 

There is an article's worth of material to cover on how Constructor functions work under the hood, but I'll leave that for another day. Today is about `new`. 

The most important thing to take from this section is that the Constructor function, when invoked with the `new` keyword, will return an *object* with a `doors` property, a `color` property, and a `drive` method.

### **class**
The `class` keyword was introduced to Javascript with the ES2015 specification, commonly known as ES6, soon to be known as "just Javascript."

The `class` keyword introduces nothing new (ha) -- it just provides some syntactical sugar for folks who like Java and semantic keywords. Nothing wrong with that. 

Here's how you use it:

```javascript
class Car {
    constructor(doors=4, color='red') {
        this.doors = doors;
        this.color = color;
    }

    drive() { console.log('Vroom!'); }
    // or drive = () => console.log('Vroom!');
}

```

Notice anything familiar?

I'll give you a hint: 
```javascript
console.log(typeof Car) // Function 
```

### Under the Hood

In this section, I'm just going to explain the things that the `new` keyword is doing with human words. Then in the next section, I'll show it in computer words. 

<!--
OUTLINE 
[x] Brief intro to Constructor functions

[x] Brief intro to class keyword

[] Javascript's `new` keyword does some interesting stuff under the hood (4 things - bulletted list)

[] reimplementing it in code 

[] why it does these things

[] where the new keyword came from 

[] why Constructor functions / classes might not be the best idea (instanceof, extends, class inheritance, tight coupling and rigid hierarchies) and how `new` can be a red flag

[] however, all of that being said, it is possible to use classes and sleep at night. 

[] so take this information with a grain of salt and always remember: use the right tool for the job and be consistent. 
-->