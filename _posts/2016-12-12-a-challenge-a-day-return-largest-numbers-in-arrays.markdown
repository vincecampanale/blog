---
layout: post
title:  "A Challenge A Day: Return Largest Numbers in Arrays"
date:   2016-12-12
categories: jekyll update
---

Today's challenge hails from [Free Code Camp](https://www.freecodecamp.com/)'s "Basic Algorithm Scripting" section. The task is this: given an input array containing several arrays of numbers, return an array containing only the largest number from each input array. For example, `largestNumbers([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]])` should return `[5, 27, 39, 1001]`.

#### Step 1: Glare at problem. Do not break eye contact.
Go on, I'll wait.

#### Step 2: Determine approach. Write pseudocode.
```
// 1) Initialize a new array to hold the largest numbers.
//            (Called "largestNumbers" perhaps?)
// 2) Iterate through the input array
//  2a) Find the maximum number in each of the inner arrays
//  2b) Push that number to our "largestNumbers" array
// 3) Return "largestNumbers"
// 4) Cha cha real smooth
```
{: .language-javascript}

#### Step 3: Translate pseudocode into real code.
First, let's handle the first two pieces of our pseudocode. We want to instantiate a new array to contain the largest numbers, then create a for loop that will step through the input array and allow us to run code on each inner array.
```
function getLargestNumbers(arr){
  var largestNumbers = [];
  for (var i = 0; i < arr.length; i++){

  }
}
```
{: .language-javascript}

Okay, now let's figure out the code we want to execute on each array.
There are many ways to find the maximum number in an array, but today we are just going to use [Math.max](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/max)[.apply()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply). The .apply() method takes two parameters. The first parameter is the context on which the code will execute. To learn more about context, check out [this great article by Ryan Morr](http://ryanmorr.com/understanding-scope-and-context-in-javascript/). The context of `Math.max()` is Math, so passing Math in as the first parameter to `.apply()` just makes sure this remains consistent. However, since `.max()` does not require context, this is *technically* unnecessary. The second parameter of `.apply()` is an array-like object on which the method will be applied. In our case, this is the inner array accessed in our for loop. So, let's put this to use and write the code to get the largest number in each inner array and save that number in a variable for later use.
```
var maximum = Math.max.apply(Math, arr[i]);
```
{: .language-javascript}
Then, we want to push that maximum to `largestNumbers`.
```
largestNumbers.push(maximum);
```
{: .language-javascript}

#### Step 4: Put it all together.
Time to add the maximum-finding code inside the for loop and return `largestNumbers` at the end.
```
function getLargestNumbers(arr) {
  var largestNumbers = [];
  for (var i = 0; i<arr.length; i++){
    var maximum = Math.max.apply(Math, arr[i]);
    largestNumbers.push(maximum);
  }
  return largestNumbers;
}
```
{: .language-javascript}

We can make this a bit more concise and eliminate an extra variable like so:
```
function getLargestNumbers(arr){
  var largestNumbers = [];
  for (var i = 0; i<arr.length; i++){
    largestNumbers.push(Math.max.apply(Math, arr[i]));
  }
  return largestNumbers;
}
```
{: .language-javascript}

And we're done!
