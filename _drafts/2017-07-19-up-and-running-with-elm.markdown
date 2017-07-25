---
layout: post
title:  "Get Up & Running with Elm"
description: "A brief introduction to Elm and step-by-step instructions to get a hello world going."
date: 2017-07-17
---

Welcome! I'm excited that you're excited about getting started with Elm. Elm is great. It's hip, cool, a little funky, and genuinely powerful. Let's get started.

### What *is* Elm?

So the folks over at [Pragmatic Studio](https://pragmaticstudio.com/) gave a great one sentence answer to this question: "Elm is a functional programming language that compiles to JavaScript and runs in the browser." 

The basic building block of Elm is the function. Every Elm application is built from a bunch of functions composed together and wired up in such a way that data flows from user interactions, through a store-like architecture, and changes are presented to the view. The program "reacts" to the user's actions. In this way, Elm could be called a functional "reactive" language. It really depends who you ask though, "reactive" is kind of a fuzzy word. 

### What's it good for?

Functions in Elm are *pure*. This means that they just turn inputs to outputs and do nothing else (no side effects). A program composed of pure functions leads to deterministic state management, time travel debugging, and easy testability. The presence of a strong typing and a static compiler means no run-time errors. Bundle these features into a compile-to-Javascript language and you get a sleek, minimalistic toolbox for building responsive, flexible, and maintainable web applications that don't fail at run-time. Yea. Wow.

### How do I get started?!

`npm install -g elm`

### More Resources