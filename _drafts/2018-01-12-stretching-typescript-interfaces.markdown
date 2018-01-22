---
layout: post
title:  "Can I dynamically add properties to an interface in Typescript?"
description: "Exploring options for making Typescript interfaces more flexible."
date: 2018-01-12
---

No, you cannot dynamically change / create an interface as it is a *static* value, used for static, structural type checking by the Typescript compiler.
However, you can get pretty creative with an interface and chances are you can mold it on the fly to fit your use-case.

I'm going to assume some basic knowledge about Typescript's duck typing and interfaces. If that's not you, no worries -- [check out these docs](https://www.typescriptlang.org/docs/handbook/interfaces.html) and circle back to this article when you're ready.

### Interfaces are like contracts

A good analogy for a Typescript interface is a contract. A contract binds the signer to a specific set of guidelines and if those guidelines are not followed, there are repercussions. When you use an interface, you are telling the Typescript compiler that any data labelled with that interface will structurally resemble the interface. If you tell the compiler you are going to produce data with a specific shape, but the data you produce does not actually fit that shape, the compiler will throw an error. Another important feature of a contract is that once it is written and signed, it is sealed. The core contract itself cannot be changed. But it can be amended. The purpose of this article is to show some techniques for *amending* existing interfaces, such that they can be reused for data that is related to or closely resembles the original interface.

Let's define a basic interface to use as an example throughout this article.

```
interface Question {
  content: string;
  answer: string;
  points: number;
}
```

Now, if I create a variable and type it as a `Question`, it better have all three of those properties with the correct types. If it doesn't, the compiler will throw an error.

### Use `extends`

Let's say that you are building a quiz application. Originally, it was just a question and answer type of quiz, so the basic `Question` interface worked fine. Now, you want to add multiple choice questions. Since the `Question` interface only allows one answer, you can't use it out of the box. Cue `extends`.

The `extends` keyword allows you to create a new interface or type that is based on an existing interface. In order to support multiple choice questions, let's create a new interface `MultipleChoiceQuestion`:

```
interface MultipleChoiceQuestion {
  content: string;
  answer: string;
  points: number;
  answerOptions: string[];
}
```

Notice how this interface is *nearly* identical to `Question`, it just has one extra property. This is a perfect time to use `extends` because you're new interface will always have all the properties that your base interface has, with at least one additional property.

```
interface MultipleChoiceQuestion extends Question {
  answerOptions: string[];
}
```

Now, if you add properties to `Question`, they will automatically get added to `MultipleChoiceQuestion` because `MultipleChoiceQuestion` *inherits* everything from `Question`. This can be a bad thing ([gorilla banana problem](https://www.google.com/search?q=gorilla+banana+problem&oq=gorilla+banana+problem)). Essentially, the use of `extends` results in tight-coupling between the inherited interface, `Question` in this case, and all the interfaces extending it. Imagine you wanted to add a property to `Question` that you didn't want on `MultipleChoiceQuestion` ... if you find yourself in this situation, just don't use `extends` for that case. It's no longer the appropriate tool.

In this particular case, since we *know* that `MultipleChoiceQuestion` will always be a form of `Question` and *must* have all the properties `Question` has, it's okay.

Let's look at some other options.

### Intersection types

You can think of Intersection types as sort of "a la carte" extensions. It's like ordering a meal and then ordering another side as an afterthought. Or keeping up with the contract metaphor, while `extends` is sort of like making a copy of the contract with more terms included, an intersection type is more like an on-the-fly exception or extra term added to the contract for a very specific reason (not sure what the legal term for this would be).

Using the quiz example, let's say there is only one question in the entire application that requires a hint. Since it's only a one time thing and it's not worth creating yet another interface to represent `QuestionWithHint`, you can do this:

```
let questionWithHint: Question & { hint: string };
```

Now, `questionWithHint` must have all the properties of `Question` with an extra property `hint`. If you needed to use this pattern more that two times, you should make a new interface for it.
