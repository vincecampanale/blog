---
layout: post
title:  "Solving 'Leap' in Elm" 
description: "A short article about using basic utilities from Elm's core libraries to solve a simple problem." 
date: 2017-08-16
---

I'm very new to Elm (a couple of weeks) and am working through exercises on [exercism.io](exercism.io). Writing about what I learn usually helps me understand it better. Hence, this article was born.

The exercise is simple: identify leap years with Elm. So, given a year, determine whether that year is a leap year or not. The rules are as follows:  

```
leap years are on: 
- every year that is evenly divisible by 4
- except every year that is evenly divisible by 100
- unless the year is also evenly divisible by 400
```

Let's start by writing some functions for checking these divisibility rules.

```elm
divisibleBy4 year = year % 4 == 0
divisibleBy4 year = year % 100 == 0
divisibleBy4 year = year % 400 == 0
```
Now for the boolean expression...

```elm
(divisibleBy4 year) && (not (divisibleBy100 year) || (divisibleBy400 year))
```

Cool. So now we can put the pieces together and build our function. 
But first, a function signature!

We want a function that takes an integer (`Int`) and returns a boolean (`Bool`): 
```elm
isLeapYear : Int -> Bool
```

Bam.

Now, we can add a `let-in` expression to create a local function scope for our `divisibleByX` helpers. 

```elm
isLeapYear year =
  let
    divisibleBy4 y = 
        y % 4 == 0

    divisibleBy100 y =
      y % 100 == 0

    divisibleBy400 y =
      y % 400 == 0
  in 
    (divisibleBy4 year) && (not (divisibleBy100 year) || (divisibleBy400 year))
```

This could actually be much better.

First off, we can make `divisibleByX` a lot more useful by having it take two arguments rather than one: the number to divide and the number to divide by.

```elm
divisibleBy n y =
 y % n == 0
```
Nice, but there's more. One of Elm's core libraries, "Basics" has a function called `rem` with the following type signature:
```elm
rem : Int -> Int -> Int
```
According to the docs, 
it does this:

```
Find the remainder after dividing one number by another.

e.g. rem 11 4 == 3
```

Is this exactly what we need? Yes, this is exactly what we need.

```
divisibleBy n y = rem y n == 0
```

Noice. Time to refactor.
```
isLeapYear : Int -> Bool
isLeapYear y =
    let
        divisibleBy n y =
            rem y n == 0
    in
        (divisibleBy 4 y) && (not (divisibleBy 100 y) || (divisibleBy 400 y))
```

That boolean expression looks like it could be simplified. There's another interesting boolean operator in the Basics package...`xor`.
```
xor : Bool -> Bool -> Bool

The exclusive-or operator. True if exactly one input is True.
```

Cool beans. Let's refactor our solution.

If we read our requirements again, this problem actually lends itself quite well to `xor`. 
We want all years divisible by 4 EXCEPT the ones divisible by 100. 

```
xor (divisibleBy 4 year) (divisibleBy 100 year) || (divisibleBy 400 year)
```

This leaves us with a third iteration of:
```
isLeapYear : Int -> Bool
isLeapYear year =
    let
        divisibleBy n y =
            rem y n == 0
    in
        xor (divisibleBy 4 year) (divisibleBy 100 year) || (divisibleBy 400 year)
```

But surely there is a better way...






