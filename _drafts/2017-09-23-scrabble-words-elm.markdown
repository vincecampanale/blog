---
layout: post
title:  "How to Score Scrabble Words with Elm" 
description: "A total noob solves a very important problem with smelly, yet functional Elm code." 
date: 2017-09-23
---

So I've been learning Elm in my spare time lately. It's a neat language with great learning resources and I'm learning a lot about the functional paradigm in the process. In this post, I'm going to walk through my solution to a simple challenge on exercism.io using Elm.

#### The Problem

For ages, man has been vexxed by a simple-seeming, yet insidious problem. It originated in an ancient mind game presented by Cleobulus of Lindos, one of the Seven Sages of Greece<sup>1</sup> called "Scrabble" (originally pronounced 'scra - blay'<sup>2</sup>). While the game has it's "rules", different cultures and societies have addressed this problem in the best way they see fit, but a universal solution has yet to arise. Today, I aim to provide that solution.

Without further ado, the problem statment: "Given a word, compute the scrabble score for that word."

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







<sup>1</sup> I made this up.  
<sup>2</sup> Also made up.  