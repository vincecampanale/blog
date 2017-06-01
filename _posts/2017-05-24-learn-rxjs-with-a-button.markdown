---
layout: post
title:  "Learn RxJS with a Button"
description: "Several core concepts of RxJS delivered via a simple button."
date: 2017-05-24
---
[ POST UNDER CONSTRUCTION -- READ ON, BUT BEWARE OF CLIFFHANGERS! ]

Welcome. The goal of this post is to teach you the fundamentals of Reactive Extensions for Javascript (RxJS). I will only scrape the surface of this very cool library to give you a sense of what you can do with it, but there is so much more to learn. 

I'm still getting a grasp on using RxJS in production myself, so if you are reading this with experience and have feedback, please hit me up on Twitter (handle in footer) or email me -- don't hold back! If you're completely new to RxJS, don't worry, I have made no assumptions about prior knowledge in this post. 

I'm going to build on the first example introduced in [this talk](https://www.youtube.com/watch?v=5CTL7aqSvJU) by Lukas Ruebellke.

Clone [this repo](https://github.com/vincecampanale/learn-rxjs-with-a-button) to get the seed locally. You can also `checkout` the `completed` branch to see the end result (along with a bonus feature not covered in this guide 🕵️). 

You don't need to know Angular to follow along, just follow the instructions in the README, open `src/app/app.component.ts` and you're good to go. There will be a comment in the `ngOnInit()` method in the `AppComponent` class -- replace that comment with the code as I cover it line-by-line. I encourage you to experiment and see what other cool streams you can make as we progress. 


<h2>The Button</h2> 

The part of the code we will be interacting with is in the `template` property of the root component.

I've also provided it here in case you don't feel like cloning the project and installing / serving it: 

```html
<button #btn md-raised-button color="accent">
    Button
</button>

<div class="container">
    <h1>{ { messages } }</h1>
</div>
```

Here we have a button and a message.  

We are going to listen for click events on this button and update the message when the button is clicked. 

### Creating a Click Stream 🐟

Just as a stream of water runs downhill, time flows in one direction, continuous and uninterrupted. Now, imagine a rock dropping into a flowing stream. There would be a splash. RxJS allows you to respond to UI events just as a stream responds to a falling rock. 

As an example, let's model click events on a particular button as a stream. 

Here's a handy diagram:  
```
------x-----x-----x--->
```
The arrow here represents time, you could think of each `-` as a discrete moment. Let's pretend that this stream represents a button sitting on the screen. As time passes, a user may or may not click on the aforementioned button. Each `x` indicates that the user has clicked on the button, thus firing a 'click' event. 

```javascript
const rxBtn = this.getNativeElement(this.btn);       // get the button element
const click$ = Observable.fromEvent(rxBtn, 'click'); // listen for clicks
```

That's not so bad. We're creating a click stream, which is an `Observable` (don't worry too much about that for now, but do take a second to think about what an `Observable` is just based on it's name).

**Note:** A common convention when working with Observable streams is to end your stream variables with `$`. It's basically an abbreviation for "stream" -- e.g. `clickStream` becomes `click$`.

### RxJS Operators

Operators are the methods that we have access to when working with Observables. RxJS operators encourage *declarative programming*, meaning that instead of telling the computer *how* to do what you want (i.e. `for` loops), you just tell it *what* you want (i.,e. `map( from this => to that )`).

##### [Begin Tangent]

A brief example of using *declarative* programming to double numbers in an array: 

```javascript
// not declarative :( 
const a = [1, 2, 3];
const double = arr => {
    for ( let i = 0; i < arr.length; i++ ) {
        arr[i] = arr[i] * 2;
    }
    return arr; 
}
double(a); // [2, 4, 6]
```


```javascript
// declarative :) 
const a = [1, 2, 3];
const double = arr => arr.map( x => x * 2 );
double(a); // [2, 4, 6]
```

Side note: There's another difference between these two blocks -- the latter returns a new array, the former just mutates the original array. Always prefer the approach *without* mutation. 

##### [End Tangent]

Okay, back to the task at hand. 

If you go up to the top of the `app.component.ts` file, you'll see several `import` statements that look like this: 

```javascript
import 'rxjs/add/observable/fromEvent';
import 'rxjs/add/observable/timer';

import 'rxjs/add/operator/filter';
import 'rxjs/add/operator/map';
import 'rxjs/add/operator/debounceTime';
import 'rxjs/add/operator/buffer';
```

These are all the operators we will use in this example. 

Let's start by taking our click stream and splitting it up into segments of 250 milliseconds. This gives our user plenty of time to double click, but not too much, so they won't get impatient. In order to do this, we're going to compose two useful operators: `debounceTime()` and `buffer()`. 

#### **debounceTime()**

The first step to segmenting our clickStream (`click$`) is to debounce based on time between inputs. In other words, when the user clicks, we start a timer that goes for 250 milliseconds. If the user clicks again while that timer is running, the timer will begin again. The debounced stream will not *emit* until that timer runs to completion (250 milliseconds pass without clicks from the user). 

In code, it will look something like this: 
```javascript
const debounced$ = click$.debounceTime(250);
```

If you `console.log` the `debouncedClicks$` like so: 

```javascript
debounced$.subscribe(console.log);
```

...you should see...

```console
MouseEvent {isTrusted: true, screenX: 3046, screenY: 239, clientX: 161, clientY: 132…}
```

...in the console.

As you can see, we give the user time to get their double click in, but only one event is emitted! So, how do we collect the clicks that got debounced?

#### **buffer()**

Buffer works like this:

Let's say this is our `click$` event stream (the arrow is time, `x`s are clicks). 

```
-----x---x-------x----x---x-x----x->
```

Buffer will collect output values until the *provided observable* "emits." So we need to give `buffer()` an *observable* as our first argument. Buffer will then collect output values into a bucket until that provided observable "emits," at which point it will set that bucket aside and begin collecting a new bucket. It just so happens that we have a `debounceTime()` event emitting after 250 milliseconds of silence post-click event. Let's collect all the click events that happen during that 250 mililisecond window into a bucket. 

```
   *   = `debounced$` observable emits

   ==  = 250 milliseconds

--x--> = `click$` observable

|____| = `buffer` bucket


        ==*      ==*         ==* ==*
-----x--x--------x------x---x----x----->
     |____|      |_|    |______| |_|


```

Note that the buckets end when `debouncedClicks$` emits. 

Now, the code should be easy to understand. If it's not, tweet at me (not a joke, save me some embarassment).

```javascript
const buffered$ = clicks$.buffer(debounced$);
```

Reviewing what we have so far in code:

```javascript
const rxBtn = this.getNativeElement(this.btn);       // get the button element
const click$ = Observable.fromEvent(rxBtn, 'click'); // listen for clicks

const debounced$ = click$.debounceTime(250); // debounce the click stream
const buffered$ = click$.buffer(debounced$); // buffer the debounced stream
```

The next step is to find a way to count the number of clicks in each bucket so we can pinpoint bucket with two clicks.

#### **map()** 🗺

Not to be confused with [`Array.prototype.map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map?v=example), this is [`Observable.map()`](http://reactivex.io/documentation/operators/map.html). It does the same thing as `Array.prototype.map()`, but with ~observables~. 

In this step, we're going to do something simple, yet powerful. 

Each buffered bucket is an array of `MouseEvents` (clicks in this case). If I quickly click the button three times in a row, it looks like this:

```javascript
buffered$.subscribe(console.log); // [MouseEvent, MouseEvent, MouseEvent]
```
Just like any Javascript array, this array has a `.length` property, which we are going to use to count the number of clicks in this bucket. 

Let's create a function that takes an array and returns its length:

```javascript 
const toLength = a => a.length;
```

We can apply this to our buffered click stream to get the number of clicks in each bucket:

```javascript
const clickCount$ = buffered$.map(toLength);
```

Great. We have converted our buckets of clicks into counts. But, we still have not isolated *double* clicks. 

#### **filter()**

Imagine we have an array of numbers `a  = [1, 2, 3, 2, 2, 1]` and we want to only keep the `2`s and move them to a new array. Our `filter()` call would look like `a.filter(x => x === 2)`. 

Well, observables have a `filter()` too!

```javascript
const doubleClick$ = clickCount$.filter(x => x === 2);
``` 

The resulting observable (`doubleClick$`) will now only emit when the user double clicks on the button!

Now we can respond to this event and update the message!

### **subscribe()** 

I've already shown `.subscribe()` in action earlier in this post -- back in the `debounceTime()` and `buffer()` sections I used it to log the contents of a the `debounced$` and `buffer$` observable streams to the console. Similar to a magazine, you won't receive any content from an observable stream unless you *subscribe* to it.

We want to subscribe to our `doubleClick$` observable and respond to it's events by updating the message to say `"Double click!"`.  

```javascript
doubleClick$.subscribe(event => this.message = "Double click!");
```

That's it! It's really that easy. No, this is not a trap. 

Notice that we are mapping the double-click event to something completely unrelated. The event itself isn't useful to us, just knowing that it occured is what we need. What we do with that event when it occurs is completely up to us. While what we're doing here is technically a side-effect and there's a whole can o' worms there, I'm just going to ignore that and focus on the fact that we can do *whatever* we want with this observable stream once we get ahold of it.

To wrap everything up, here's the entire block of code we have constructed throughout this guide: 

```javascript 
const toLength = a => a.length; // helper -- gets length of given array

const rxBtn = this.getNativeElement(this.btn);       // get the button element
const click$ = Observable.fromEvent(rxBtn, 'click'); // listen for clicks

const debounced$ = click$.debounceTime(250); // debounce the click stream
const buffered$ = click$.buffer(debounced$); // buffer the debounced stream

const clickCount$ = buffered$.map(tolength);            // get buffer lengths
const doubleClick$ = clickCount$.filter(x => x === 2);  // filter for length 2

doubleClick$.subscribe(event => this.message = "Double click!");
```

Note: observable methods can be chained and composed just like any other Javascript methods.
Sometimes it's nice to have your streams partitioned for reusability and cleanliness, but sometimes it's also nice to eliminate intermediate variables.

Check it:

```javascript
const rxBtn = this.getNativeElement(this.btn);       // get the button element
const click$ = Observable.fromEvent(rxBtn, 'click'); // listen for clicks

click$
    .buffer(click$.debounceTime(250))
    .map(a => a.length)
    .filter(x => x === 2)
    .subscribe(e => this.message = "Double click!");
```


#### Bonus Challenges:
1) Build a function that takes a number and a click stream and returns a new stream containing clicks of that number (i.e. `filterClicks(click$)(3)`) returns a stream of triple clicks. [[Currying](http://www.vincecampanale.com/blog/2017/04/22/what-is-currying/) is optional but encouraged!]  
2) Respond to shift-click events.  
3) Make a clear button to clear the message (using observables!). 



Post in progress... stay tuned...



<!--
    let clickStream = Observable.fromEvent(this.getNativeElement(this.btn), 'click');

    let streamLength = clickStream
      .filter(event => !event.shiftKey)
      .buffer( clickStream.debounceTime(250) )
      .map( list => list.length );

    let singleClicks = streamLength
      .filter ( x => x === 1 );
    singleClicks.subscribe(event => this.message = "Clicked!");
    let doubleClicks = streamLength.filter ( x => x === 2 );
    doubleClicks.subscribe(event => this.message = "Double Clicked!!");
    let tripleClicks = streamLength.filter( x => x === 3 );
    tripleClicks.subscribe(event => this.message = "TRIPLE Clicked!!!");



    const shiftKey = clickStream
      .filter( event => event.shiftKey )
      .map( event => '~ Shift Clicked ~' );

    shiftKey.subscribe( message => this.message = message );
-->



#### To Learn More
* This post was inspired by my meanderings through Lukas Reubellke's course [Hello RxJS](https://courses.ultimateangular.com/p/hello-rxjs).
* Lukas also gave a great [talk](https://www.youtube.com/watch?v=5CTL7aqSvJU) on RxJS, mentioned at the top of this post.  
* Andre Staltz wrote an excellent, in-depth gist on Reactive Programming: [The introduction to Reactive Programming you've been missing](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754).
