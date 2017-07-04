---
layout: post
title:  "Solved: Check for Palindromes"
date:   2016-12-10
---
Another day, another challenge. This one comes from the Free Code Camp "Basic Algorithm Scripting" section. I highly recommend checking out Free Code Camp's program if you are getting started on learning to code. It has served me very well.

Today, as the name implies, we are checking for palindromes. A palindrome is a word, sentence, or phrase that reads the same when spelled backwards or forwards. For example, `race car` is a palindrome.
Our function will be called `palindrome` and the input will be a string. If the input is a palindrome, our function will return true; otherwise, it will return false.

#### Step 1: Create the function.
```
function palindrome(str){

}
```
{: .language-javascript}

Great. Now let's put some meat (or veggies) on this sandwich.

#### Step 2: Stare the problem down. Let it know who's boss. Then psuedocode.
This challenge is harder than it seems at face value. Let's make sure it doesn't know that, though.
A handy hint that Free Code Camp gives us is "Note: You'll need to remove all non-alphanumeric characters (punctuation, spaces, and symbols) and turn everything into lower case in order to check for palindromes."
Let's break this down into some pseudocode:
```
1. Remove all non-alphanumeric characters from the input string
2. Make it lower case
3. Make a backwards version of the cleaned up string
4. Compare the cleaned up string to its backwards version
  4a. If the two are the same, return true. End function.
  4b. If the two are not the same, return false End function.
```

#### Step 3: Clean up the input.
Time to implement the first two steps in the above pseudocode.
First, step 1: remove the non-alphanumeric characters from the input string.
Good ol' JavaScript has a good method we can use for this called [String.prototype.replace()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/replace). This method will search the string it is called upon for the given substring or regular expression in the first argument, and replace it with whatever you put in the second argument.
Instead of weeding out all non-alphanumeric characters by referencing them directly in a long, unwieldy regular expression, we are just going to say replace everything that is NOT an alphanumeric character by adding the `^` in front. It would be convenient if we could just say `^\w` or `\W`, but unfortunately we must also get rid of underscores. I've gotta say, Regular Expressions give me headaches.

```
var replaced = str.replace(/[^a-zA-Z0-9]/g, '');
```
{: language-javascript}
So now we have a new string called `replaced` which contains only alphanumeric characters from the input string. Now let's make that lowercase.
```
var lowercase = replaced.toLowerCase();
```
{: language-javascript}
Here's what we have so far:
```
function palindrome(str){
  var replaced = str.replace(/[^a-zA-Z0-9]/g, '');
  var lowercase = replaced.toLowerCase();
}
```
{: language-javascript}

#### Step 4: Reverse the lowercase string and compare.
Check out [this post](http://www.vincecampanale.com/jekyll/update/2016/12/08/a-challenge-a-day-reverse-a-string/) to see about reversing a string. I'm just going to reuse that code here.
```
//save the cleaned up string in a variable called "forwards"
var forwards = lowercase;
//reverse lowercase and save it in a variable called "backwards"
var backwards = lowercase.split('').reverse().join('');
```
{: language-javascript}

Now for the comparing part. We'll just use an if statement here.
```
if(forwards === backwards){
  return true;
}
```
{: language-javascript}

That's it. Easy peasy.
You might be thinking "Wait, Vince, dude, what about the 'else return false' part?"
Astute observation, dear Reader. Bear with me.

#### Step 5: Put it all together.
Our functional function:
```
function palindrome(str){
  var replaced = str.replace(/[^a-zA-Z0-9]/g, '');
  var lowercase = replaced.toLowerCase();
  var forwards = lowercase;
  var backwards = lowercase.split('').reverse().join('');
  if (forwards === backwards){
    return true;
  }
  return false;
}
```
{: language-javascript}

The reason we don't need an else statement here is because JavaScript will read the code within this function each line before the next. When it hits the if statement and evaluates the equality of forwards and backwards, and finds that they are in fact equal, the function returns true and then ends. When the return statement fires, it also *ends the function*. Nothing after the return statement happens. So, if `forwards` is exactly the same as `backwards`, the function returns true and ends. If `forwards` is *not* exactly the same as `backwards`, then it skips the code in the if statement, and returns false.

#### Step 6: Refactor.
The function functions, but it could be cleaner.
```
function palindrome(str) {
  var forwards, backwards, result;
  forwards = str.replace(/[^a-zA-Z0-9]/g, '').toLowerCase();
  backwards = forwards.split('').reverse().join('');
  result = forwards === backwards ? true : false;
  return result;
}
```
{: language-javascript}

This solution takes advantage of a ternary function which is basically syntactic sugar for the if statement. The line `result = forwards === backwards ? true : false` does this: evaluates `forwards === backwards`; if it evaluates to true, then true gets stored in the variable `result`; if it evaluates to false, then false gets stored in the variable `result`.


#### Step 7: Refactor. Again.

The informed reader will recognize some code smells in the snippet above. A ternary expression that returns true or false? Bah. Creating three variables to do just one expression? Bah. ES5 in 2017?? Baah!!

Check this out \*cracks knuckles\*...

```javascript
const palindrome = str => str.replace(/[^a-zA-Z0-9]/g, '').toLowerCase() === forwards.split('').reverse().join('');
```

Boom.


Let me know if this helped (or not)! [Follow me on Twitter](https://twitter.com/_vincecampanale) or [Github](https://github.com/vincecampanale) for more beginner-friendly Javascript posts like this one.

Fin.
