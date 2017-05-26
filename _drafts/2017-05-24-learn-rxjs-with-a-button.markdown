---
layout: post
title:  "Learn RxJS with a Button"
description: "Several core concepts of RxJS delivered via a simple button."
date: 2017-05-24
---
<h2>The Button</h2> 

I'm going to build on the first example introduced in [this talk](https://www.youtube.com/watch?v=5CTL7aqSvJU) by Lukas Ruebellke.

Clone this repo to get the seed locally. You can also `checkout` the `completed` branch to see the end result (along with a bonus feature not covered in this guide 🕵️). 

You don't need to know Angular to follow along, just open `src/app/app.component.ts` and get going! There will be a comment in the `ngOnInit()` method in the `AppComponent` class -- replace that comment with the code as I cover it line-by-line. See what other cool streams you can make as we progress. 

The part of the code we will be interacting with is in the `template` property. 

{% highlight html %}
<button #btn md-raised-button color="accent">
    Button
</button>
<div class="container">
    <h1>{{message}}</h1>
</div>
{% endhighlight %}

Here we have a button and a message. 
We are going to listen for click events on this button and update the message when the button is clicked. 

### Creating a Click Stream 🐟

Just as a stream of water runs downhill, time flows in one direction, continuous and uninterrupted. Now, imagine a rock dropping into a flowing stream. There would be a splash. RxJS allows you to respond to UI events just as the stream responds to the falling rock. 

Let's take user clicks on a button for example.

Another way to look at a stream is with one of these handy diagrams:  
```
    ------#-----#-----#--->
```
The arrow here represents time, you could think of each `-` as a discrete moment. Let's pretend that this stream represents a button sitting on the screen. As time passes, a user may or may not click on said button. The `#` indicates that the user has clicked on the button, thus firing a 'click' event. 

Here's how you make that arrow diagram with RxJS:
{% highlight javascript %}
    const clickStream = Observable.fromEvent(this.getNativeElement(this.btn), 'click');
{% endhighlight %}





{% highlight javascript %}
{% endhighlight %}

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


To Learn More:  
    This post was inspired by my meanderings through Lukas Reubellke's course Hello RxJS.  
    Andre Staltz wrote an excellent, in-depth article on Reactive Programming.  
