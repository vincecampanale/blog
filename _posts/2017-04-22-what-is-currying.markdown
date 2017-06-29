---
layout: post
title:  "What is Currying?"
description: "A concise, need-to-know explanation of 'currying' in Javascript."
date: 2017-04-22
---

For the record, this article is more for personal reference than anything. I am still in the midst of learning the basics of functional programming (FP). This is my attempt to pin down my understanding of currying, a fundamental concept in FP. 

Outline in parts:  

1 What does a *not* curried function look like?  
2 What does a curried function look like?  
3 Demonstrate a major benefit of currying: reusability  

Here goes nothin'

#### 1 A "Function" That Is Not "Curried" 👍

{% highlight javascript %}
  const mooltiplyTwo = (a, b) => a * b;
  mooltiplyTwo(4, 2); // 8
{% endhighlight %}

Wonderful.

#### 2 Curried 🏀

{% highlight javascript %}
  const swoosh = a => b => a * b;
  swoosh(4)(2); // 8
{% endhighlight %}

Excellent.

Immediately we can deduce a couple of differences, just from the function invocations:  
* the curried function has a very clever name  
* the non-curried function gets all its parameters between the same set of parentheses, separated by commas  
* the curried function gets its parameters one at a time, each one invoked by itself (wrapped in its own set of parentheses)  
  * almost like a curried function is a chain of little functions 🤔   

In Javascript, parentheses mean we are invoking a function, so `mooltiplyTwo(4)` must be a function itself if we can invoke it with another set of parentheses (this is exactly what is happening). Replacing `a` with `4`, we see that `const swoosh = a => b => a * b;` is the equivalent of `const swoosh = b => 4 * b;`.  

The best part is we can actually save `swoosh(4)` in a variable, like so: 
```javascript
const times4 = swoosh(4); // Function (a => 4a)
```
And then *re*use it whenever:
```javascript
const eight = times4(2); // 8
```

We can do this because in Javascript, functions are first class 🥇.

#### 3 One of many major benefits of currying: **Reusability**

We had a multiply function (`mootiplyTwo(a, b)`) already. *Currying* that function allowed us to skip re-inventing the wheel in order to multiply a number by 2. 

Imagine this on a much larger scale, applied to many more functions, which can then be composed into bigger, more powerful functions. Functions that manage the entire state of the application for example.  <sub>(\*cough\* Redux)</sub>


There is so much more to learn. [Follow me on twitter](https://twitter.com/_vincecampanale) for updates on my learnings about things like function "arity" and partial application coming up. Stay tuned. ✌️
