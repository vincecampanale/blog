---
layout: post
title:  "Elm Kata: Strain"
description: "Implementing List.keep and List.discard functions in Elm."
date: 2018-01-30
---

The prompt:
> Implement the `keep` and `discard` operation on collections. Given a collection and a predicate on the collection's elements, `keep` returns a new collection containing those elements where the predicate is true, while `discard` returns a new collection containing those elements where the predicate is false.

### Implementing `keep`

Implementing `keep` is trivial. It's essentially another name for [`List.filter`](http://package.elm-lang.org/packages/elm-lang/core/5.1.1/List#filter).

The type signature is straight-forward, it's the same as `List.filter`. We want to accept two arguments: 1) a "predicated" function which accepts any type and returns a boolean and 2) a list of the same generic type that the predicate accepts. That signature looks like this:

```
keep : (a -> Bool) -> List a -> List a
```

Then the actual function just calls `List.filter` -- easy peasy.

```
keep fn list =
    List.filter fn list
```

### Implementing `discard`

The implementation for `discard` is a bit more complicated. The type signature is the same as before:

```
discard: (a -> Bool) -> List a -> List a
```

But now, some extra work is required to determine which elements stay. The elements that are *not* in the list resulting from `List.filter` should stay. First, I'll create an internal helper function to determine whether an element is a member of the list filtered based on the provided predicate. We want to discard those items.

```
discard fn list =
    let
        isInList el =
            if List.member el (List.filter fn list) then
                // this item should be discarded...
            else
                // this item should be kept
    in
        // TBD...
```

Okay, now we need a function that works kind of like filter, but the opposite. We sort of want to `map` our original list to a new list without the items in the result of `(List.filter fn list)`. It seems like [`filterMap`](http://package.elm-lang.org/packages/elm-lang/core/5.1.1/List#filterMap) will do the trick. The docs say that it will *"Apply a function that may succeed to all values in the list, but only keep the successes"*. In our case, we want to keep the items that fail the member check above.

```
discard fn list =
    let
        isInList el =
            if List.member el (List.filter fn list) then
                Nothing
            else
                Just el
    in
        List.filterMap isInList list
```

And that'll do it. But this could probably be improved. Time for the best part.

### Refactoring `discard`

