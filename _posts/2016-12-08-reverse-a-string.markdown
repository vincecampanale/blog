---
layout: post
title:  "Solved: Reverse a String"
date:   2016-12-08 00:30:00
---
The challenge I'm going to solve today comes from Free Code Camp's "Basic Algorithm Scripting". My task is this: given a string, reverse it. I believe Missy Elliot once said something along the lines of, "I put a string down, flip it and reverse it." I dedicate this challenge to her.

I solved this challenge a few months ago when I first started learning to code and the solution is saved there in Free Code Camp for me to cringe at. I'll share it with you, so you may partake in the cringe-fest.

#### Solution 1 (Total Noob)
```
function reverseString(str){
  var charArray = [];
  for (i = 0; i < str.length; i++){
    charArray[i] = str[i];
  }

  charArray.reverse();
  strReversed = charArray.join('');

  return strReversed;
}
```
{: .language-javascript}

So here's how it works. First, I create a dummy array. Then, I step through each character of the input string and loads the character into the array. Example: `"hello"` becomes `["h", "e", "l", "l", "o"]`. Next, it calls the [.reverse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) method on that array.  Finally, it calls the [.join()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join) method on the array with no spaces between the characters (that's what the empty string argument does), stores the resulting string in a variable aptly named strReversed, and spits out strReversed. Another neat thing to note with this solution is that I'm taking advantage of something very cool about JavaScript: Arrays are dynamic. This means that an array will expand as you add elements to it (and shrink as you remove them). This is a very useful feature that not every language has.

While Solution 1 works, it could be better. Here's what I would do now, with a couple more months of coding practice and a few projects under my belt.

#### Solution 2 (Less Noob)
```
function reverseString(str){
  var strReversed = str.split('').reverse().join('');
  return strReversed;
}
```
{: .language-javascript}

This much cleaner, more concise solution takes advantage of method chaining, a handy technique in JavaScript. Method chaining is hard to define without using the word in the definition, so I'm going to use the word in the definition. Method chaining is when you chain methods one after the other, each method operating on whatever the previous method returned. In this example, `str.split('')` returns an array of characters in the input string (Side Note, from [Mozilla Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split) for `.split()`: "If separator (the first parameter) is an empty string, str is converted to an array of characters." -- this is a neat trick and very handy). Then, `.reverse()` operates on that array and, as in the previous solution, reverses it. Finally, `.join('')` joins the reversed array. The result of all these methods is then saved in the variable strReversed, which is returned. It's considered best practice to separate the logic from the return statement when possible, however this could technically be a one-liner:

`return str.split('').reverse().join('')`.

Hope this was helpful! If you have questions, concerns, suggestions, or just want to say "sup," feel free to shoot me an email or tweet at me. Contact info is in the footer of this site.

Til next time! Adios.
