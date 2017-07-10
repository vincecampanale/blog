---
layout: post
title:  "Solved: Confirm the Ending"
date:   2017-03-19
---

"Confirm the Ending" is another great problem from Free Code Camp's algorithm scripting challenges. In the words of the Free Code Camp folks, "The goal is to check if a string (first argument, `str`) ends with the given target string (second argument, `target`)".

As is common for coding exercises, this challenge can be solved countless ways. Today, I'm going to walk through my solution from when I first started coding, explain the concept of extending a prototype in Javascript, describe the difference between `.substr()` and `.substring()`, two built-in array methods on the Javascript `String` prototype, and finally, provide a memory-efficient, one-line solution to the problem.  

#### Beginner Solution

When I had first ventured into learning to code, I took a stab at this problem and successfully solved it, however my solution is a little..."verbose"...  

I decided to use the `.substr()` method which takes two arguments. The first argument is the index in the string to *start* at and the second argument number of characters to extract from that index. So, say you want to get `"lo"` from `"hello!""`. In order to do this with `.substr()`, you would say `"hello!".substr(3, 2)`. This will go to the third character in `"hello"!`, which is the first `"l"`, then go for one more character before stopping at the fourth character in `"hello!"`, which is `"o"`. If you're thinking my counting skills need some work, which they probably do, remember that in Javascript, indices are 0-based. So the character `"h"` has index 0 in the string.

{% highlight javascript %}
function confirmEnding(str, target) {
  var substr = str.substr(str.length - target.length, target.length);

  if (substr === target) return true;
  else return false;
}
{% endhighlight %}

Just to briefly touch on what I did here: create a variable containing a string chopped off the end of the `str` argument that has the same length as the target string. If the string contained in that variable matches the target, return true. Otherwise, return false. Pretty simple solution to a pretty simple problem but this can be way better.

#### Better Solution

There's a neat trick you can use when calling `.substr`. If you give it a negative number as it's first and only argument, e.g. `"string".substr(-3)`, you will get a string containing the characters starting at the given number *from the end of the string*. So `"string".substr(-3)` returns `"ing"`. Let's employ this trick in our solution:

{% highlight javascript %}
function confirmEnding(str, target) {
  return target === str.substr(-target.length);
}
{% endhighlight %}

In English, this function says: return a string with the same length as `target` from the end of `str` and compare the new string to `target`. If they are *strictly* equal, return true. Otherwise, return false. This will work with two equal signs as well, but it's typically good practice to use the triple equals.

#### And another one.

```javascript
const confirmEnding = (str, tar) => tar === str.substr(-tar.length);
```

#### The Difference Between `.susbtr()` and `.substring()`

This feat can also be accomplished with `.substring()`, however I don't think the solution is quite as elegant. It's a good exercise though, and I'll leave it to you to try it out. These methods may appear to be aliases for one another, however they actually do different things.

From the MDN docs (aka the Javascript Bible):

`.substr()`: "returns the characters in a string beginning at the specified location through the specified number of characters".  
`.substring()`: "returns a subset of a string between one index and another, or through the end of the string".  

Perhaps an easier way to think about it is this:

`.substr()`: starting at index *argument one*, get *argument two* number of characters.  
`.substring()`: starting at index *argument one*, get the characters up until *argument two*.  

Yet another way to look at it:

`.substr(from this index, for this many characters)`  
`.substring(from this index, to this index)`  

Hope this was helpful! Reach out anytime with feedback, questions, tips, and especially compliments -- my twitter handle and email address are in the footer.

Vinny out. 
