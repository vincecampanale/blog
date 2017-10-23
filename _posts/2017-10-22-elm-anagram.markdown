---
layout: post
title:  "Elm Kata: Using Elm Core List and String Functions to Detect Anagrams" 
description: "In this segment of Elm Kata, we solve the exercism.io exercise 'Anagram' with Elm first using the community package 'List.Extra', then refactoring to use just core libraries." 
date: 2017-10-22
---

Welcome to the third installment of **Elm Kata**, a series in which we learn the ins and outs of Elm by solving different exercises.

### The Problem

Another day, another problem to solve. Call me Sherlock. Here's the problem statement we're working with, courtesy of exercism.io:  

> Given a word and a list of possible anagrams, select the correct sublist.  
Given "listen" and a list of candidates like "enlists" "google" "inlets" "banana" the program should return a list containing "inlets". 

### Round 1

As per usual, I like to start out with a function signature so let's start there:
```
detect : String -> List String -> List String
```

We are going to create a function that takes a word and a list of words and returns a new list containing all the anagrams of the original word in the given list (if any).

My first thought upon seeing this problem was "permutations." If I could generate a list of all the combinations of letters in the given word and check that list for each of the words in the given list of possible candidates, I'd end up with a list of anagrams.  

I'll show that solution now, but keep in mind there's a much better way. Kudos if you can see it already, it took me the full process of solving the problem the hard way before I saw the easy way -- I feel like Confucius would nod knowingly if he heard me say that. 

Here's the first pass in all its glory:

```
module Anagram exposing (..)

import List.Extra exposing (permutations)


getPermutationsOf : String -> List String
getPermutationsOf word =
    word
        |> String.toList
        |> List.Extra.permutations
        |> List.map String.fromList


isPermutationOf : String -> String -> Bool
isPermutationOf word other =
    let
        uppercaseWord =
            String.toUpper word

        uppercaseOther =
            String.toUpper other
    in
        not (uppercaseWord == uppercaseOther)
            && List.member uppercaseOther (getPermutationsOf uppercaseWord)


detect : String -> List String -> List String
detect word possibleMatches =
    possibleMatches
        |> List.filter (isPermutationOf word)

```

Just a heads up, if you want to run this yourself you will need to run `elm-package install elm-community/list-extra` to get access to the fancy permutations method. 

So, allow me to walk you through this solution. Let's walk backwards, starting with the final function and unraveling the program.

In `detect`, I filter the list of `possibleMatches` for words that are a permutation of the word for which I am seeking anagrams.  In order to do this, I needed a helper function to tell me if a word is a permutation of another word, hence the second function in the program. 

`isPermutationOf` takes two words and returns whether or not the second word is present in the permutation list of the first one. First I convert the two words to upper case (to handle edge cases and weird characters), then I make sure that the two words are not the exact same (another edge case), and finally I check the permutation list for the second word. 

Getting the permutation list takes a bit of work as well, though. Hence, the first function in the program. `getPermutationsOf` is where I actually use the `List.extra.permutations` utility. I take the word, convert it to a list of characters, turn that into a list of lists (each list containing a possible order list of characters could be in) and then map over that list of lists, turning it back into a list of strings. 


The result is a very slow, very brute force, but totally correct solution.

### Round 2

How did I not see it in the first place? Me-oh-my, this can be done much more cleanly-er.

Instead of changing the original word to make the check easy for the list, why don't we change all the words in the list to make the check easy for the word?!

How can we check that two words are the same, just with the letters all jumbled up? By sorting them and comparing their sorted forms!

First, we need a sort function that will work for strings:
```
sortStr = String.toList >> List.sort >> String.fromList
```

The `>>` was new for me -- it lets you compose functions together to make bigger, badder functions. It's similar to the pipe (`|>`) operator, except you aren't passing in any arguments, you're just mashing the functions together in anticipation of a higher purpose.

Now we need to put our `sortStr` function to use in a function that will take two words and see if they are anagrams!

```
isAnagram word possibleMatch =
  not (String.toUpper word == String.toUpper possibleMatch)
    && sortStr (String.toUpper word) 
    == sortStr (String.toUpper possibleMatch)        
```

Let's use our helpers to make a clever `detect` function that makes our old `detect` function look like baby food.

```
detect : String -> List String -> List String
detect word possibleMatches =
    let
        sortStr =
            String.toList >> List.sort >> String.fromList

        isAnagram possibleMatch =
            not (String.toUpper word == String.toUpper possibleMatch)
                && sortStr (String.toUpper word)
                == sortStr (String.toUpper possibleMatch)
    in
        List.filter isAnagram possibleMatches
```

Boom, there ya have it.

Follow [@_vincecampanale](https://twitter.com/_vincecampanale) to get updates on future installments of the **Elm Kata** series. Also, it will make me feel good about myself.

Thanks for your time <3, hope it was worth it.
