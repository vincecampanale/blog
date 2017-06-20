---
layout: post
title:  What is Javascript's `new` keyword doing under the hood?
description: "A description of how Javascript's Constructor function, class keyword, and new keyword work together, followed by a reimplementation of the new keyword in Javascript."
date: 2017-06-07
---

Good morning, afternoon, evening, or night. I have some things to share with you about the `new` keyword in Javascript. Important things. 

I'll start with some context and background about Constructor functions and the `class` keyword. Then, I will explain exactly *what* the `new` keyword is doing under the hood. Next,
 I will show *how* it is doing what it does by implementing it in code. Finally, I will explain *why* it does these things and give a couple arguments for avoiding this approach to Javascript object creation altogether in *most* situations. The information presented here comes from [these](https://www.youtube.com/watch?v=Y3zzCY62NYc) [resources](https://medium.com/javascript-scene/javascript-factory-functions-vs-constructor-functions-vs-classes-2f22ceddf33e) and several others, processed by my brain. 

### Constructor functions 🛠
A Constructor function is a function that builds and returns a new instance of object. It looks like this: 

```javascript
/** Car: {
*    doors: number,
*    color: string,
*    drive: Function
*   }
*
* Car(doors: number, color: string) => Car
*/

function Car(doors=4, color='red') {
    this.doors = doors;
    this.color = color;
    this.drive = () => console.log('Vroom!');
}
```

The capital letter at the beginning of the Constructor name is simply a convention adopted by Javascript programmers to separate *Constructor* functions from regular functions. 

The way Constructor functions work under the hood might make for an interesting article, but I'll leave that for another day. Today is about `new`. 

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

### Under the Hood 🚗

Whether you are using a vanilla Constructor function or a <strike>unnecessary</strike> special keyword to instantiate your object constructing mechanism, you will be using the `new` keyword. (There is another secret and powerful way to generate objects in Javascript called a [factory](https://medium.com/javascript-scene/javascript-factory-functions-vs-constructor-functions-vs-classes-2f22ceddf33e) function which will have to be covered in a future post).

So what is the `new` keyword doing under the hood (in human words)?

Three letters, four actions. When you say `var myCar = new Car()`, it...

```
1) Creates a new (empty) object 
2) Gets the prototype of the constructor function (Car) and sets it as the empty object's prototype
3) Calls the constructor function with the new empty object as `this` 
4) Return the new object
```

What does this process look like in computer words?

*Note:* In order to reimplement `new` we will have to pass in the constructor and it's arguments separately. 

First, let's do it in ES5 because you only live once. 

```javascript
// new(constructor: Function, constructorArgs: Array<any>) => Object
function new2(constructor, constructorArgs) {

    // Step 1: Create an empty object
    var newObject = {};

    // Step 2a: Get the prototype of the constructor function
    var constructorPrototype = constructor.prototype;
    // Step 2b: Set the empty object's prototype 
    Object.setPrototypeOf(newObject, constructorPrototype);

    // Retro technique to turn arguments into an actual array 
    var argsArray = Array.prototype.slice.apply(arguments); 
    // Slice off first argument b/c that's the constructor function itself. 
    var realConstructorArgs = argsArray.slice(1);
    
    // Step 3: Invoke constructor with newObject as 'this'
    constructor.apply(newObject, realConstructorArgs);

    // Step 4: Return the new object :)
    return newObject;
}
```
Now that we have a working implementation, we can clean it up and make use of some new tools from ES6. 

```javascript
// new(constructor: Function, constructorArgs: Array<any>) => Object
function new2(constructor, ...constructorArgs) {
    const newObject = {};
    Object.setPrototypeOf(newObject, constructor.prototype);    
    constructor.apply(newObject, constructorArgs);
    return newObject;
}
```

And...
```javascript
const myCar = new2(Car, 4, 'blue');
console.log(myCar) // { doors: 4, color: 'blue', drive: [Function] }
myCar.drive() // Vroom!
```

*But wait*, there is an edge case. If the constructor function itself returns a new object, like this...

```javascript
function Car(doors, color) {
    this.doors = doors;
    this.color = color;
    this.drive = () => console.log('Vroom!');
    return {
      doors,
      color
    }
}
```
 we should just return that object directly:

```javascript
// new(constructor: Function, constructorArgs: Array<any>) => Object
function new2(constructor, ...constructorArgs) {
    const newObject = {};
    Object.setPrototypeOf(newObject, constructor.prototype);
    return constructor.apply(newObject, constructorArgs) || newObject;
}
```

And we're done.


Hope this helped! 

Tweet me with feedback [@_vincecampanale](https://twitter.com/_vincecampanale) if it did or didn't.

Til next time 👋.
