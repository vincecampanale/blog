---
layout: post
title:  "Learn RxJS with a Button"
description: "Several core concepts of RxJS delivered via a simple button."
date: 2017-05-24
---

Welcome. The goal of this post is to teach you the fundamentals of Reactive Extensions for Javascript (RxJS). I will only scrape the surface of this very cool library to give you a sense for what you can do with it, but there is so much more to learn. 

I'm still getting a grasp on using RxJS in production myself, so if you are reading this with experience and have feedback, please hit me up on Twitter (handle in footer) or email me -- don't hold back! If you're completely new to RxJS, don't worry, I have made no assumptions about prior knowledge in this post. 

I'm going to build on the first example introduced in [this talk](https://www.youtube.com/watch?v=5CTL7aqSvJU) by Lukas Ruebellke.

Clone [this repo](https://github.com/vincecampanale/learn-rxjs-with-a-button) to get the seed locally. You can also `checkout` the `completed` branch to see the end result (along with a bonus feature not covered in this guide 🕵️). 

You don't need to know Angular to follow along, just open `src/app/app.component.ts` and you're good to go. There will be a comment in the `ngOnInit()` method in the `AppComponent` class -- replace that comment with the code as I cover it line-by-line. I encourage you to experiment and see what other cool streams you can make as we progress. 


<h2>The Button</h2> 

The part of the code we will be interacting with is in the `template` property of the root component.

I've also provided it here in case you don't feel like cloning the project and installing / serving it: 

    <button #btn md-raised-button color="accent">
        Button
    </button>
    
    <div class="container">
        <h1>{ { messages } }</h1>
    </div>


As you can see, we have a button and a message.  

We are going to listen for click events on this button and update the message when the button is clicked. 

### Creating a Click Stream 🐟

Just as a stream of water runs downhill, time flows in one direction, continuous and uninterrupted. Now, imagine a rock dropping into a flowing stream. There would be a splash. RxJS allows you to respond to UI events just as the stream responds to the falling rock. 

As an example, let's model click events on a particular button as a stream. 

Here's a handy diagram:  
```
------x-----x-----x--->
```
The arrow here represents time, you could think of each `-` as a discrete moment. Let's pretend that this stream represents a button sitting on the screen. As time passes, a user may or may not click on said button. Each `x` indicates that the user has clicked on the button, thus firing a 'click' event. 

Here's how you make that arrow diagram with RxJS:

    const rxBtn = this.getNativeElement(this.btn);
    const clickStream = Observable.fromEvent(rxBtn, 'click');

This code reads pretty smoothly. We're creating a click stream, which is an `Observable` (don't worry too much about that for now, but do take a second to think about what an `Observable` is just based on it's name).

### RxJS Operators

Operators are the methods that we have access to when working with Observables. RxJS operators encourage *declarative programming*, meaning that instead of telling the computer *how* to do what you want, you just tell it *what* you want.

##### [Begin Tangent]

A brief example of using *declarative* programming to multiply numbers in an array by 2: 

    // not declarative :( 

    const a = [1, 2, 3];
    const double = arr => {
        for ( let i = 0; i < arr.length; i++ ) {
            arr[i] = arr[i] * 2;
        }
        return arr; 
    }
    double(a) // [2, 4, 6]

<br />

    // declarative :) 

    const a = [1, 2, 3];
    const double = arr => arr.map( x => x * 2 );
    double(a) // [2, 4, 6]

Side note: There's another difference between these two blocks -- the latter returns a new array, the former just mutates the original array. Always prefer the approach *without* mutation. 

##### [End Tangent]

Okay, back to the task at hand. 

If you go up to the top of the `app.component.ts` file, you'll see several `import` statements that look like this: 

    import 'rxjs/add/observable/fromEvent';
    import 'rxjs/add/operator/filter';
    import 'rxjs/add/operator/map';
    import 'rxjs/add/operator/debounceTime';
    import 'rxjs/add/operator/buffer';
    import 'rxjs/add/observable/timer';

These are all the operators we will use in this example. 


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

To Learn More:  
    * This post was inspired by my meanderings through Lukas Reubellke's course Hello RxJS.  
    * Andre Staltz wrote an excellent, in-depth article on Reactive Programming.
