---
layout: post
title:  "Using Recursion to Loop in Elm"
description: "A simple problem that would normally be solved with a for loop solved in Elm (a language with no for loop) using recursion."
date: 2018-07-04
---

This post is centered on the following problem: 

```
Find the difference between the square of the sum and the sum of the squares of the first N natural numbers.

The square of the sum of the first ten natural numbers is (1 + 2 + ... + 10)² = 55² = 3025.

The sum of the squares of the first ten natural numbers is 1² + 2² + ... + 10² = 385.

Hence the difference between the square of the sum of the first ten natural numbers and the sum of the squares of the first ten natural numbers is 3025 - 385 = 2640.
```

Credit for the problem goes to [exercism.io](exercism.io/exercises/elm/difference-of-squares/readme).

The plan is:   
```
1) solve it with a `for` loop in Javascript,   
2) solve it with recursion in Javascript,  
3) translate the recursive solution to Elm  
```

### With a `for` Loop 

The `for` loop solution goes like this:

```
-- get the square of the sum of n by:
  -- going from 1 to n
  -- and adding each number to a total
-- return the total after the loop is done

-- get the sum of the squares of n by:
  -- going from 1 to n
  -- and adding the square of each number to a total
-- return the total after the loop is done

-- subtract the latter from the former
```

In code, it looks something like the following. 

```javascript
function squareOfSum(number){
  let sum = 0;
  for (let i = 1; i <= number; i++) {
    sum += i;
  
  }
  return Math.pow(sum, 2);

}

function sumOfSquares(number) {
  let sum = 0;
  for (let i = 1; i <= number; i++) {
    sum += Math.pow(i, 2);
  
  }
  return sum;

}

function difference(number) {
  return squareOfSum(number) - sumOfSquares(number);

}

console.log(difference(10) === 2640); // true
```

Thanks to my extensive test suite, I can confidently refactor and use recursion instead.

### In Order to Understand Recursion...

The recursive equivalent of the above solution goes like this:

```
-- get the square of the sum of n by:
  -- getting the triangular number for n by:
    -- returning 0 if n is 0
    -- adding n to the triangular number of n - 1

-- get the sum of the squares of n by:
  -- returning 0 if n is 0
  -- adding the square of n to the sum of the squares of n - 1

-- subtract the latter from the former
```
So, recursion is acting as a different way of looping by defining an action for each number `n` down to 1 and a final action to end the loop when `n` gets to 0.

I googled "factorial with adding instead of multiplying" and found ["triangular numbers"](https://en.wikipedia.org/wiki/Triangular_number), so the function for calculating the sum of positive integers from 1 to `N` is called `triangulate` 🤷🏻‍♂️.

Let's write that function first:

```javascript
function triangulate(n) {
  if (n === 0) {
    return 0;
  } else {
    return n + triangulate(n - 1);
  }
}

// which can be simplified to:

function triangulate(n) {
  return n === 0 ? 0 : n + triangulate(n - 1);
}
```

Using the triangulate function, we can get the `squareOfSum` function: 

```javascript
function squareOfSum(n) {
  const sum = triangulate(n);
  return Math.pow(sum, 2);
}
```

The `sumOfSquares` function can also use recursion:

```javascript 
function sumOfSquares(n) {
  if (n === 0) {
    return 0;
  } else {
    return Math.pow(n, 2) + sumOfSquares(n - 1);
  }
}

// again, can be reduced to..

function sumOfSquares(n) {
  return n === 0 ? Math.pow(n, 2) + sumOfSquares(n - 1);
}
```

One final thought on the Javascript solution is to make `triangulate` a bit more generic and add a second parameter for an exponent.

```javascript
const triangulate = (n, exp = 1) => 
  n === 0
  ? 0
  : Math.pow(n, exp) + triangulate(n - 1, exp);
```

Then `sumOfSquares` can be written as the following:

```javascript
function sumOfSquares(n) {
  return triangulate(n, 2);
}
```



### How about some Elm?

Elm doesn't have `for` loops. Whaaaaa

Yea, for real.

Luckily, we already know this problem can be solved without a `for` loop. So what's the Elm equivalent of the recursive solution above? Well, let's refactor `sumOfSquares` just *one* more time in Javascript, using a switch statement with only two cases this time.

```javascript
function sumOfSquares(n) {
  switch (n) {
    case 0:
      return 0;
    default:
      return Math.pow(n, 2) + sumOfSquares(n - 1);
  }
}
```

Elm has a `case` statement, so a nearly equivalent function will work:

```elm
sumOfSquares : Int -> Int
sumOfSquares n =
  case n of
    0 -> 0
    _ -> (n ^ 2) + sumOfSquares (n - 1)
```

We can apply a similar approach to `squareOfSum`:

```elm
squareOfSum : Int -> Int
squareOfSum n = 
  let
    triangulate x =
      case x of
          0 -> 0
          _ -> x + triangulate (x - 1)
  in 
    (triangulate n) ^ 2
```

Then the final function `difference` is just:

```elm
difference : Int -> Int
difference n =
  (squareOfSum n) - (sumOfSquares n)
```

And voila, we have solved a `for`-loop-friendly problem in Elm, a language with no `for` loop. 

### A Better Way?

While we *can* use recursion to loop through the numbers between `0` and `N`, we can also make use of other utilities exposed in [Elm Core](http://package.elm-lang.org/packages/elm-lang/core/latest/).

For example, `List.range` and `List.sum` make this problem much easier.

```elm
import List exposing (map, range, sum)


square : Int -> Int
square n =
    n ^ 2


squareOfSum : Int -> Int
squareOfSum n =
    range 1 n |> sum |> square


sumOfSquares : Int -> Int
sumOfSquares n =
    range 1 n |> map square |> sum


difference : Int -> Int
difference n =
    squareOfSum n - sumOfSquares n
```

Since `for` loops are one of the first things we learn as programmers, it's easy to fall back on `for` loops in solutions to everyday problems. Using Elm has taught me that `for` loops aren't necessary most of the time and seeking a different solution can lead to more declarative and readable code.

Thanks for reading :)

