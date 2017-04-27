---
layout: post
title:  "What is Currying?"
description: "A concise, need-to-know explanation of 'currying' in Javascript."
date: 2017-04-22
---

For the record, this article is more for personal reference than anything. I am still in the midst of learning the basics of functional programming. This is my attempt to pin down my understanding of **currying**. I'm going to break down my explanation into five parts:  

    1) I'll start by creating a function that is NOT curried.  
    2) Then, I'll curry that function and analyze the difference.  
    3) Show how you can save pieces of a curried function in variables and reuse them later.  

Okay, here goes.

### A "Function" That Is Not "Curried" 👍

{% highlight javascript %}
  const goForthAndMultiply = (a, b) => a * b;
  goForthAndMultiply(4, 2); // 8
{% endhighlight %}

Wonderful.

### Curry (no steph 🏀)

{% highlight javascript %}
  const goForthAndCurry = a => b => a * b;
  goForthAndCurry(4)(2); // 8
{% endhighlight %}

Excellent.

A simple way to look at this is we're now giving our arguments one at a time in parentheses rather than commas.
Parentheses mean we are invoking a function, so `goForthAndCurry(4)` must actually be a function. And looking closely
at the code, it is! Replacing `a` with `4`, you can see that `goForthAndCurry(4)` is the equivalent of `const goForthAndCurry = b => 4 * b;`.  

The best part is you can actually save `goForthAndCurry(4)` in a variable because in Javascript, functions are first class.
Allow me to expand on this point in the next section.

### But Why? ¯\\_(ツ)_/¯

Because:  

    1) Reusable.  
    2) Functions.  

Check this out.

{% highlight javascript %}
  const multiply2 = goForthAndCurry(2);
{% endhighlight %}

That line of code actually returns a *function* which will multiply a given number by 2. Used like so:  

{% highlight javascript %}
  multiply2(21); // 42
{% endhighlight %}

We had a multiply function (called `goForthAndMultiply`) already. Since we were smart and *curried* that function, we didn't have to re-invent the wheel just to multiply a number by 2. That sounds like a *reusable* *function* to me.



There is so much more to learn. I'll be posting more about other cool things like function "arity" and partial application in the future. Stay tuned. ✌️
