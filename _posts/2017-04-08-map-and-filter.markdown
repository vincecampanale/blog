---
layout: post
title:  "The Beauty of Javascript's Map & Filter"
date:   2017-04-08
---
Functional programming has been a hot topic as of late. I'm not going to cover much about FP in this article, but if you want to learn more (and you should), just type 'Eric Elliot functional programming' into The Almighty Google and start reading.

Map & filter are two crucial functions in functional programming. You don't have to know anything about functional programming to know how to use them and I guarantee you that these two little functions _will_ improve your code.

Let's learn by example. This exercise is taken straight from a tutorial I'm currently working through called "Functional Programming in Javascript," which is intended to serve as an introduction to RxJs (Reactive Extensions). Check it out when you're done reading this article: [http://reactivex.io/learnrx/](http://reactivex.io/learnrx/). I'm going to solve the same problem in six different ways in order to show the true beauty of `.map()` and `.filter()`.

Here we have some data containing a list of newly released movies in JSON format:

{% highlight javascript %}
//Excellent test data from http://reactivex.io/learnrx/
 var newReleases = [
     {
       "id": 70111470,
       "title": "Die Hard",
       "boxart": "http://cdn-0.nflximg.com/images/2891/DieHard.jpg",
       "uri": "http://api.netflix.com/catalog/titles/movies/70111470",
       "rating": 4.0,
       "bookmark": []
     },
     {
       "id": 654356453,
       "title": "Bad Boys",
       "boxart": "http://cdn-0.nflximg.com/images/2891/BadBoys.jpg",
       "uri": "http://api.netflix.com/catalog/titles/movies/70111470",
       "rating": 5.0,
       "bookmark": [{ id: 432534, time: 65876586 }]
     },
     {
       "id": 65432445,
       "title": "The Chamber",
       "boxart": "http://cdn-0.nflximg.com/images/2891/TheChamber.jpg",
       "uri": "http://api.netflix.com/catalog/titles/movies/70111470",
       "rating": 4.0,
       "bookmark": []
     },
     {
       "id": 675465,
       "title": "Fracture",
       "boxart": "http://cdn-0.nflximg.com/images/2891/Fracture.jpg",
       "uri": "http://api.netflix.com/catalog/titles/movies/70111470",
       "rating": 5.0,
       "bookmark": [{ id: 432534, time: 65876586 }]
     }
   ];
{% endhighlight %}

Each movie has several properties: `id`, `title`, `boxart`, `uri`, `rating`, and `bookmark` (an array of JSON objects). In this tutorial, I'm going to solve a simple problem: _use map and filter to collect the ids of movies with 5.0 ratings_.

#### For the Love of Loops
The first way I will solve this problem makes use of our oldest friend, the humble `for` loop.

{% highlight javascript %}
var counter,
  otherCounter,
  favorites = [],
  favoriteIds = [];

for ( counter = 0; counter < newReleases.length; counter++ ) {
  if ( newReleases[counter].rating === 5.0 ) {
    favorites.push(newReleases[counter]);
  }
}

for ( otherCounter = 0; otherCounter < favorites.length; otherCounter++ ) {
  favoriteIds.push(favorites[otherCounter].id);
}

{% endhighlight %}

Lovely. This gets the job done, but I have three problems with this code:  

    1) There's a lot of code here to do a simple task.  

    2) We're making a lot of variables to track our values, which is pretty wasteful memory-wise.  

    3) Does it really matter that we traverse the movie list from the beginning to end? Couldn't we do it in any order? Do we really *need* to explicitly spell that out in our code?  

  Ask yourself number three and really sit and contemplate that for a moment. When we use the `for` loop to tell an iterator to traverse an array, we have to spell out in code the _order_ in which the array is traversed. This is useful sometimes, but most of the time we're just going from beginning to end -- smells like abstraction time.

#### For Each or Not For Each
`.forEach()` abstracts the explicit logic of the `for` loop away. We just call `.forEach()` on our `newReleases` array and trust that the computer will traverse the array. The computer can traverse the array beginning to end, end to beginning, middle out, upside-down, it really doesn't matter. The point is: _we don't have to tell the computer about how the array is traversed -- we just know we're going to do something on each element of the array_. That's where the **reducer function** comes in. The reducer function is our instruction to the computer about _what_ should happen when the iterator encounters each element in the array. For example, let's say we want to check if a movie has a rating of 5 starts and push it to an array if it does. Our function would look like this:

{% highlight javascript %}
function (movie) {
  if ( movie.rating === 5.0) {
    favorites.push(movie);
  }
}
{% endhighlight %}

By passing this function as a reducer to `.forEach()`, we run it on ever element in the array.

{% highlight javascript %}

var favorites = [],
    favoriteIds = [];

newReleases.forEach(function(movie) {
  if ( movie.rating === 5.0 ) {
    favorites.push(movie);
  }
});

favorites.forEach(function(movie) {
  favoriteIds.push(movie.id);
});

{% endhighlight %}
