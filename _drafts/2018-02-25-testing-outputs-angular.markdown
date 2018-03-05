---
layout: post
title:  "How to unit test Angular @Outputs (EventEmitters)"
description: "A brief guide on testing EventEmitters in Angular components."
date: 2018-02-25
---

Angular was built for testability. Powerful tools like dependency injection, the `TestBed` API, and out-of-the-box integration with Jasmine make testing Angular apps a joy. The catch is that learning these API's can take some time. Throw in a mix of Angular, Jasmine, and RxJS jargon and it can be a real uphill battle to feel comfortable testing the hairier parts of your application (which are the most important parts to test, of course). 

# What is an @Output property? 

An `@Output` property is an Angular utility used to create custom events. An `@Output` is an [`EventEmitter`](https://angular.io/api/core/EventEmitter), meaning it has two methods: `emit` and `subscribe`. You probably won't `subscribe` to it directly in a simple application, since Angular handles that with it's event binding syntax (e.g. `<div (click)="onClick($event)"></div>`). The `emit` method allows you to notify the parent of an event and pass data up.

# What should a unit test for an @Output do? 

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
