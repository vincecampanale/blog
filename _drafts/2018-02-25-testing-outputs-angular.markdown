---
layout: post
title:  "Testing Outputs (EventEmitters) in Angular with the fakeAsync Utility"
description: "A brief guide on testing EventEmitters in Angular components."
date: 2018-02-25
---

Angular was built for testability. An emphasis on separation of concerns, modularity, and dependency injection, combined with powerful API's like the `TestBed` and easy-to-use libraries like Jasmine make testing in Angular not only possible, but truly powerful. You can test *everything* in your Angular application. The catch is that learning these API's can take some time. Throw in a mix of Angular, Jasmine, and rxjs jargon and it can be quite an uphill battle to feel comfortable testing the hairier parts of your application (which are the most important parts to test, of course). 

In this article, I give a brief overview of what custom outputs / `EventEmitters` are then I go into way too much detail about how to test them.  Hopefully, some of it is immediately applicable to your app, while even more of it gives you something to chew on.

# What is an @Output property? (3 sentences)
-- It's an Angular for making custom events
-- Under the hood: EventEmitter 
 * has a couple of methods
 * implementation looks roughly like this: ...
-- Similar to how you can listen to a (click) event on a component
 * the @Output property allows you to emit custom events from child components and listen to them with the event binding syntax (click) in the parent 

# What should a unit test for an @Output do? (5 sentences)
-- from the perspective of the component doing the emitting
 * ensure that the function is emitting the correct data
-- from the perspective of the component listening for the event
 * ensure that the function is using the emitted data correctly
 
# Testing the Emitter
-- Technique 1: Spy on the `emit` method (~10 sentences)
-- Technique 2: Create a mock host component and listen for the event from the child

# Testing the Parent
-- Just call the function directly (trust that Angular will work)
-- Or, you can mock the child component and directly emit the event from it (since the output is a public property)
