--- 
layout: post
title:  "My First Two Weeks Using VIM"
description: "I've heard VIM is the bees knees. Time to find out for myself."
date: 2017-08-01
---

<a href="https://imgflip.com/i/1teh21"><img src="https://i.imgflip.com/1teh21.jpg" title="made at imgflip.com"/></a>

I have been putting this off for a while. I have always admired people that use Vim. I view them as sort of "super devs". If they use Dvorak, I have to pull out my shades to keep from being blinded by their genius. In this article, I will document the beginning of my journey to become as bright as the sun and faster than lightning.

### Days 1 & 2

My first day was actually yesterday, but I've decided to start the journal today so let's catch up. I started out doing what any good developer would do and googled 'Learn VIM'. Lo and behold, I received millions of results in split seconds. Hail the almighty Google.

I found a course by a company called Upcase that basically said: go do `vimtutor`. This entails going to your terminal (with Vim installed) and typing `vimtutor`. That's it. I started with it and spent a lot of time experimenting. I have to say, it's a little unnatural. I get a little sweaty using Vim because I'm resisting my natural tendencies so much. Don't judge me, I sweat a lot. That said, I can already see the benefits of having millions of combinations of commands literally at your fingertips. Here's a couple tips from my first couple of days:

#### Tip 1

Do not use arrow keys. Ever. This is an anti-pattern in VIM. The `hjkl` method of moving the cursor allows you to keep your hand on the home row of the keyboard, which is crucial for efficiency.
Here's a nice little diagram from the `vimtutor`.  
```
 ** To move the cursor, press the h,j,k,l keys as indicated. **
             ^
             k              Hint:  The h key is at the left and moves left.
       < h       l >               The l key is at the right and moves right.
             j                     The j key looks like a down arrow.
             v
```

#### Tip 2

By the same token, avoid using `DEL` to delete characters as much as possible. Just navigate to the character you want to delete and press `x`. Using the delete key is similar to using the arrow keys. It defeats the entire purpose of Vim.

#### Tip 3

Enable Vim shortcuts in your favorite editor. This will allow you to keep the familiar environment with color coded stuff and whatnot. I found this much more enjoyable than just looking at my terminal. There are definitely plugins and stuff that can add colors and handy tools to your terminal, but the resources I've looked at so far warn against installing a shit load of plugins right off the bat.

#### Tip 4 

Don't be too hard on yourself. I just pressed the delete key to delete something, right before I used `CTRL + Z` to undo. This is a frustrating difficult time, the first two weeks of Vim. I'm only on Day 2 and I'm feeling the burn already. If I can do this, so can you.

### Day 3

Alrighty - new day, new tips.  Many thanks to those of you who commented with tips and tricks that you've learned from your own experience with Vim. I actually really like how this post is evolving and would love to see some more tips in the comments if you have any, no matter how small or insignicant they may seem. I promise it will help someone (me).

So I ended up writing down some of the key navigation shortcuts for Vim on some post it notes and sticking them to my monitor -- here's a brief rundown of the shortcuts I have found to be essential, then I'll get into tips for the day. I've found it's useful to think of the keys as actions. The actions can be divided into different categories. The two categories I've identified are "edit" and "move". There may be more formal ways to refer to these categories, feel free to comment if you know of a better way to think of them.

```
Move commands: 

hjkl - move around (see Days 1 & 2)

w - beginning of next word
e - end of next word

b - beginning of previous word

$ - end of current line
0 - beginning of current line

Edit commands:

i (insert) - insert before the character the cursor is on
a (append) - insert after the character the cursor is on
SHIFT + A (super append) - insert after the current line

x (delete) - delete the character that the cursor is on
d (delete*) - delete ... 
```

#### Tip 5

I put a * on the delete command because it's a little special. By itself it does nothing. Combine it with a move command and you can delete all the things.

Let's say I want to delete two words that are next to each other.

```
1) Navigate to the beginning of the first word using your move commands.
2) Type d2w.
3) Press enter.
```

How cool is that?! `d` tells Vim I want to delete something. It then patiently waits for me to tell it what to delete (so polite). I say to Vim, `2w` (read: two words) and poof, they're gone. That move there shows the power of Vim. Each of these keys does something on its own and lets you run with Vim. Combine them, and you can fly.

#### Tip 6

Yesterday, I called it VIM. I called it that in the title too. I won't change it for the sake of preserving my growth as a Vim user. TIL, it's supposed to be "Vim", like "Jim".
From [`:help pronounce`](http://vimhelp.appspot.com/intro.txt.html#pronounce):
```
Vim is pronounced as one word, like Jim, not vi-ai-em.  It's written with a
capital, since it's a name, again like Jim.
```
Now I know.

#### Tip 7

This tip comes from [Jean Bernard](https://dev.to/jeanbernard) - thanks Jean!
Quoting him here:
```
Pro-tip: I use "jj" as a shortcut to the ESC key. That way when you want to switch back to normal mode you don't have to go all the way to the ESC key.
```
Love it.

I want to add a "Comfort-o-meter" to this thing. I'd say I've risen from a 1 to a 2. I no longer sweat when I use Vim, so that's progress. Still have a tendency to reach for comfortable shortcuts for selecting text and moving lines, copy-pasting, etc. However, the momentum is building on itself and I'm having fun. 

Wish me luck and we'll catch up tomorrow!

---

Post in progress...follow me on [twitter](), [github](), or [dev.to]() for daily updates :)