---
layout: post
title:  "How to Score Scrabble Words with Elm" 
description: "For ages, man has been vexxed by this simple problem. Today, I attempt a solution." 
date: 2017-09-23
---

#### The Problem

In approximately 6th century B.C., Cleobulus of Lindos, one of the Seven Sages of Greece, presented to the world a word-based mind game<sup>1</sup>. He called it "Scrabble" (originally pronounced 'scra - blay'<sup>2</sup>). When outlining his game, he omitted an important part, perhaps on purpose -- how to compute the score of a word. A universal solution has yet to arise. 

#### Assumptions

In order to provide a reasonable solution that will hold in this dimesion, we must first make some assumptions.  
```
1) The letters A, E, I, O, U, L, N, R, S, and T have the value 1.
2) The letters D and G have the value 2.  
3) The letters B, C, M, and P have the value 3.  
4) The letters F, H, V, W, and Y have the value 4.  
5) The letter K has the value 5.  
6) The letters J and X have the value 8.  
7) The letters Q and Z have the value 10.  
```

#### Solution

Let us begin.  

Breaking the problem down and writing a solution outline pseudocode tends to help with solving simple scripting problems like this one, so let's start there.

```
-- given a word
-- split the word up into letters
-- get the points value for each letter
-- store / record the point values for each word somewhere
-- add up all the point values
-- return the sum
```

I find it helpful to start with the type signature when constructing functions in Elm.  The function we want to build will take a word and return a score. In terms the compiler can understand, it will take a `String` and return an `Int`.
```
scoreWord: String -> Int
```

Next, we can start building our function.
```
scoreWord: String -> Int
scoreWord word = 
   word
```

Okay, so splitting the word up into letters is easy:

```
scoreWord: String -> Int
scoreWord word = 
  String.split "" word
```

Alrighty, now we need to get the points value for each letter. This introduces a bit complexity into our problem -- where do the point values come from? Elm doesn't know what a letter is worth, so we have to tell it. One way to do this is to make a record, a series of key-value pairs.


```
pointsCategories =
    { one = [ "A", "E", "I", "O", "U", "L", "N", "R", "S", "T" ]
    , two = [ "D", "G" ]
    , three = [ "B", "C", "M", "P" ]
    , four = [ "F", "H", "V", "W", "Y" ]
    , five = [ "K" ]
    , eight = [ "J", "X" ]
    , ten = [ "Q", "Z" ]
    }
```

Each key of our record is the word version of the point value for each letter in the list attached to it. Records in Elm cannot have number literals for keys, so this is the closest we can get.

Now, we need a function that will score a letter by looking up it's point value in the `pointsCategories` record and converting that to a number.

```
getPointsForLetter : String -> Int
getPointsForLetter letter =
    if List.member letter pointsCategories.one then
        1
    else if List.member letter pointsCategories.two then
        2
    else if List.member letter pointsCategories.three then
        3
    else if List.member letter pointsCategories.four then
        4
    else if List.member letter pointsCategories.five then
        5
    else if List.member letter pointsCategories.eight then
        8
    else if List.member letter pointsCategories.ten then
        10
    else
        0
```

Okay, a little (a lot) redundant. I'm starting to get the feeling that this program is a wee bit smelly and could use tidying up, but let's get this solution working, then we can refactor.

This would be a good time to note that all the letters in our `pointsCategories` record are uppercase, but we do not have that guarentee for a given word, unless you're playing a very loud variation of Scrabble.

So let's make our letters uppercase.

```
scoreWord : String -> Int
scoreWord word =
  List.map String.toUpper (String.split "" word)
```

Now we have a list of uppercase letters to work with -- lets use our `getPointsValue` function to convert letters to points.

```
scoreWord : String -> Int
scoreWord word = 
  List.map getPointsForLetter (List.map String.toUpper (String.split "" word))
```

These parentheses are getting out of hand -- time to take advantage of the Elm's brilliant pipe operator to make our solution readable.

The pipe operator is nice because it turns 
```
f(g(h(i(x))))
```
into
```
x 
  |> i 
  |> h 
  |> g 
  |> f
```
where `x` is some value and `i`, `h`, `g`, and `f` are functions.

Applying this nifty tool to our solution, we get:

```
scoreWord : String -> Int
scoreWord word =
  word 
    |> String.split ""
    |> List.map String.toUpper
    |> List.map getPointsForLetter
```

The astute reader will notice we are not yet returning an `Int`, but rather a list of `Int`'s (or `List Int` using Elm syntax). Let's fix that.

```
scoreWord : String -> Int
scoreWord word = 
  word 
    |> String.split ""
    |> List.map String.toUpper
    |> List.map getPointsForLetter
    |> List.sum
```

This solution works, but I detest the repetitive chain of `if else if`'s in the `getPointsForLetter` function. Unfortunately, this is the crux of the solution, so this refactor is going to be a big one!

#### Refactor

The main issue with the above solution is the fact that we can't use a number literal as the key in our `pointsCategories` record. This means we are separating looking up the value and converting it to an `Int`. We will need to reach into our toolbox and find a new data structure to use in order to solve this. I propose we use a dictionary (`Dict`) to tie a number to a character. We need to import `Dict` to use it.

```
import Dict exposing (..)

pointValues : Dict Char Int
pointValues =
    Dict.fromList
        [ ( 'A', 1 )
        , ( 'E', 1 )
        , ( 'I', 1 )
        , ( 'O', 1 )
        , ( 'U', 1 )
        , ( 'L', 1 )
        , ( 'N', 1 )
        , ( 'R', 1 )
        , ( 'S', 1 )
        , ( 'T', 1 )
        , ( 'D', 2 )
        , ( 'G', 2 )
        , ( 'B', 3 )
        , ( 'C', 3 )
        , ( 'M', 3 )
        , ( 'P', 3 )
        , ( 'F', 4 )
        , ( 'H', 4 )
        , ( 'V', 4 )
        , ( 'M', 4 )
        , ( 'Y', 4 )
        , ( 'K', 5 )
        , ( 'J', 8 )
        , ( 'X', 8 )
        , ( 'Q', 10 )
        , ( 'Z', 10 )
        ]
```
Now we can access the point value for a letter using the `Dict.get` method. This change of data structure is crucial because we have moved the redundancy out of our program and into the data itself. Redundancy in data is gravy, redundancy in code is not.

Let's write a function that takes a character and returns it's point value.

```
getPointValue : Char -> Int
getPointValue letter = 
    let
        letterScore =
            Dict.get letter pointValues
    in
    case letterScore of
        Just score ->
            score

        Nothing ->
            0
```

`Dict.get` is not guaranteed to return something becuase it can be called with a value that may not correspond to a key in the dictionary, like if we tried to get the point value of a Chinese character. In terms that Elm's compiler can understand, `Dict.get` returns a `Maybe` type, therefore we need to tell the program how to handle both cases of the `Maybe` type -- `Just` and `Nothing`.

If we get a score back, then return that score. If not, default to 0.

Time to rewrite `scoreWord` so it splits our word into a list of `Char`'s (`String.toList`) and uses our new `getPointValue` function to get the value of each character in the list.

```
scoreWord : String -> Int
scoreWord word =
    word
        |> String.toUpper
        |> String.toList
        |> List.map getPointValue
        |> List.sum
```

This works, but I still think `getPointValue` is a bit verbose. Let's use a couple tricks to make it easier to grok.

`flip` is a neat tool that lets you flip the order of arguments to a function. `Maybe.withDefault` is a shorthand way to do what we did in our `case` statement. Taking advantage of these two handy mechanisms, we can write `getPointValue` like this:

```
getPointValue : Char -> Int
getPointValue letter =
  letter 
    |> flip Dict.get pointValues
    |> Maybe.withDefault 0
```
Ah, much better.

To cap off the post, here's the entire script:
```
module ScrabbleScore exposing (..)

import Dict exposing (..)


pointValues : Dict Char Int
pointValues =
    Dict.fromList
        [ ( 'A', 1 )
        , ( 'E', 1 )
        , ( 'I', 1 )
        , ( 'O', 1 )
        , ( 'U', 1 )
        , ( 'L', 1 )
        , ( 'N', 1 )
        , ( 'R', 1 )
        , ( 'S', 1 )
        , ( 'T', 1 )
        , ( 'D', 2 )
        , ( 'G', 2 )
        , ( 'B', 3 )
        , ( 'C', 3 )
        , ( 'M', 3 )
        , ( 'P', 3 )
        , ( 'F', 4 )
        , ( 'H', 4 )
        , ( 'V', 4 )
        , ( 'M', 4 )
        , ( 'Y', 4 )
        , ( 'K', 5 )
        , ( 'J', 8 )
        , ( 'X', 8 )
        , ( 'Q', 10 )
        , ( 'Z', 10 )
        ]


getPointValue : Char -> Int
getPointValue letter =
    letter
        |> flip Dict.get pointValues
        |> Maybe.withDefault 0


scoreWord : String -> Int
scoreWord word =
    word
        |> String.toUpper
        |> String.toList
        |> List.map getPointValue
        |> List.sum
```


While we can never know exactly how Cleobulus intended for a word to be scored, we can give our best guess and this is mine.


<sup>1</sup> I made this up.  
<sup>2</sup> Also made up.  