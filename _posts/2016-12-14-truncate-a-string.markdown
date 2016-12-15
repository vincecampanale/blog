---
layout: post
title:  "Step By Step: Truncate a String"
date:   2016-12-14
---

Nothing like some good ol' Javascript string manipulation! Today, we're tackling a fun little challenge from the Free Code Camp (FCC) "Basic Algorithm Scripting" called "Truncate a string." For this challenge, we will be building a function that takes two arguments: 1) "str" - the string to be truncated and 2) "num" - the length of the resulting truncation ([it's a word](http://www.dictionary.com/browse/truncation)). However (!!), FCC has added a couple of hoops we have to jump through to make things more interesting. Hoop #1) The truncated string we return must end with `...` and note: *these dots add to the length of the string*. Hoop #2) If `num` is less than or equal to 3, then the dots *do not* add to the length of the string.

If you're thinking this is a good time to break out some conditionals, then I like the way you think, Reader (capital R).

#### Step 1: We're gonna write some psuedocode up in here, up in here

Quick shoutout to a true icon, [DMX](https://www.youtube.com/watch?v=thIVtEOtlWM). Feel free to listen to that song while you work through the rest of this post. Try not to nod your head. Try it. It's impossible. The man's a genius. I digress.

This problem is a bit tricky because we have several conditions to keep in mind. We treat strings differently based on whether or not their `length` property is greater than `num`. We also want to keep in mind whether `num` is less than or equal to 3.
Let's prioritize our conditions. There are many ways to handle this, but I'm going to go with a straightforward, easy-to-understand approach. First, we want to see whether or not `string.length` is less than or equal to `num` because that condition will change what we return. Next, let's check for the value of `num` because that also affects our return value. If neither of those conditions are met, then we can just solve the original problem.

```
// 1) If str.length is less than or equal to num
//      - just return str
// 2) Else, if num is less than or equal to three
//      - truncate the string to length `num`
//      - concatenate truncated string with `...`
// 3) Otherwise,
//      - truncate the string to length `num - 3`
//      - concatenate truncated string with `...`
```
{: .language-javascript}

#### Step 2: Translation station

Let's start with instantiating the function and outlining the conditionals.

```
function truncateString(str, num){
  var truncate; //create a variable to hold our truncated strings

  if (str.length <= num){
    //do something
  } else if (num <= 3) {
    // do something
  } else {
    // do something
  }

}
```
{: .language-javascript}

Alright, now we have our logic in place. Let's mix in the rest of the code.
To truncate the strings we're going to use the [String.prototype.slice()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/slice) method. `.slice()` returns a new string containing an extracted section of the string. The first parameter is the index at which we want to begin extracting; the second parameter is the index at which we want to stop extracting. For example `"hello".slice(0, 4)` will return ... `"hell"` ... yikes.

Anyways, let's see how `str.slice()` applies in this example.

##### Note: We can also achieve this with `.substring()` but `.slice()` sounds cooler. Try rewriting the solution with `.substring()` for good measure. Here's a [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substring) to MDN docs to get you started.

```
function truncateString(str, num){
  var truncate; //create a variable to hold our truncated strings

  if (str.length <= num) {
    return str;
  } else if (num <= 3) {
    truncate = str.slice(0, num) + "..."; //slice string from index 0 to index num
    return truncate;
  } else {
    //slice string from index 0 to index num - 3 because "..." adds to the string length when num > 3
    truncate = str.slice(0, num - 3) + "...";
    return truncate;
  }
}
```
{: .language-javascript}

This code works, but it's a bit redundant and repetitive (ha).
Let's fix it up.

#### Step 3: Improve via iteration

When learning to code, get used to iteration. And I don't just mean `for` loops. I mean get used to writing and rewriting and incrementally improving (or un-breaking) your code bit by bit (pun kind of intended). The only way to improve is to write code, and rewrite code, and rewrite the rewrites. Practice makes perfect, and practice is iteration. Now that "iteration" doesn't look like a real word anymore, let's improve our function. We'll start by eliminating redundant variables (\*cough\* truncate \*cough\*).

```
function truncateString(str) {
  if (str.length <= num) {
    return str;
  } else if (num <= 3) {
    return str.slice(0, num) + "...";
  } else {
    return str.slice(0, num - 3) + "...";
  }
}
```
{: .language-javascript}

But wait, there's more.
We can really spruce up our function with the handy dandy ternary operator.

```
function truncateString(str) {
  if (str.length <= num) {
    return str;
  } else {
    return str.slice(0 , num > 3 ? num - 3 : num) + "...";
  }
}
```
{: .language-javascript}

Nice, right? In words, this function reads "If `str.length` is less than or equal to `num`, return `str`. Otherwise, return an extraction from `str` beginning at index 0 and either ending at index `num - 3` if `num` is greater than 3 or ending at index `num` if `num` is not greater than 3, then add "..." on the end of that extraction." I really love ternary operators.

That's it for today. Feel free to email me with questions and/or suggestions.
Until next time, keep iterating.
