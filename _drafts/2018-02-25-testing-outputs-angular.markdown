---
layout: post
title:  "How do I unit test Angular custom events?"
description: "A brief guide on testing EventEmitters / @Output properties in Angular components."
date: 2018-02-25
---

Angular was built for testability. Powerful tools like dependency injection, the `TestBed` API, and out-of-the-box integration with Jasmine make testing Angular apps a joy. The catch is that learning these API's can take some time. Throw in a mix of Angular, Jasmine, and RxJS jargon and it can be a real uphill battle to feel comfortable testing the hairier parts of your application (which are the most important parts to test). 

## What is an `@Output` property? 

An `@Output` property is an Angular utility used to create custom events. An `@Output` is an [`EventEmitter`](https://angular.io/api/core/EventEmitter), meaning it has two methods: `emit` and `subscribe`. You probably won't `subscribe` to it directly in a simple application, since Angular handles that with it's event binding syntax (e.g. `<div (click)="onClick($event)"></div>`). The `emit` method allows you to notify the parent of an event and pass data up.

## What should a unit test for an @Output do? 

When testing the component with the `@Output` (the child component), the unit test should target two things: 1) the `emit` method is being called at the right time and 2) the `emit` method is emitting the expected data.

When testing the component listening to the `@Output` (the parent / container component), the unit test should aim to check that the data emitted from the event goes to the right place (e.g. passed to the correct method).

## Testing the Child

Every custom `@Output` must get triggered by another event. Whether that event be a click in the DOM, a response from the server, a custom event on yet another nested child component, there must be a cause for the `emit` method to be called. That triggering event must be mocked.

# If it's a click...

# If it's another custom event...

# If it's a response from the server...

-- Technique 1: Spy on the `emit` method (~10 sentences)
-- Technique 2: Create a mock host component and listen for the event from the child

# Testing the Parent
-- Just call the function directly (trust that Angular will work)
-- Or, you can mock the child component and directly emit the event from it (since the output is a public property)
