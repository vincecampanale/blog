---
layout: post
title:  "Solved: Symmetric Difference of Multiple Arrays (Part 2)"
date:   2017-01-05
---
This post is Part 2 of a four part series:  
[Symmetric Difference of Multiple Arrays (Part 1)]()  
Symmetric Difference of Multiple Arrays (Part 2) <- You are here  
[Symmetric Difference of Multiple Arrays (Part 3)]()  
[Symmetric Difference of Multiple Arrays (Part 4)]()  

In this section, I'm going to go over steps one and two of the strategy outlined in the previous post.  
For a brief recap, in step 1, we want to separate each argument array and create a new array in our function to store them. In step 2, we will write a function that returns the symmetric difference of two arrays.

#### Step 1: Creating Arguments Array
I'm going to show two ways to do this. The first is an older, hackier way. The second uses `.from()` which was introduced in ES2015.

First way:  
`var argArray = Array.prototype.slice.call(arguments)`

This [Stackoverflow post](http://stackoverflow.com/questions/7056925/how-does-array-prototype-slice-call-work) explains this line of code very well.
Here's the most important information from that post:
>The `.call()` and `.apply()` methods let you manually set the value of this in a function. So if we set the value of this in `.slice()` to an array-like object, `.slice()` will just assume it's working with an Array, and will do its thing.

Second way:  
`var argArray = Array.from(arguments)`

This is a much cleaner way to get an array from an array-like object. We'll use the `.from()` approach in our final solution.

#### Step 2: Finding the symmetric difference of two arrays.
This step is definitely the hardest part of the solution. We *could* use a boat load of for loops here (which I spent a long time doing the first time I solved this), but I've recently discovered the beauty of higher-order functions. So, I'm going to just skip straight to those because it's much cleaner, more concise code, and, most importantly, it's much easier to read.
