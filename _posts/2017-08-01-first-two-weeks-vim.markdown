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



---

Post in progress...follow me on [twitter](), [github](), or [dev.to]() for daily updates :)