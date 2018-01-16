---
layout: post
title:  "Can I build a dynamic interface with Typescript?"
description: "Exploring options for making Typescript interfaces more flexible."
date: 2018-01-12
---

No, you cannot dynamically change / create an interface as it is a *static* value, used for static, structural type checking by the Typescript compiler.
However, you can get pretty creative with an interface and chances are you can mold it on the fly to fit your use-case.

I'm going to assume some basic knowledge about Typescript's duck typing and interfaces. If that's not you, no worries -- [check out these docs](https://www.typescriptlang.org/docs/handbook/interfaces.html) and circle back to this article when you're ready.

### Interfaces are like contracts

A good analogy for a Typescript interface is a contract. A contract binds the signer to a specific set of guidelines and if those guidelines are not followed, there are repercussions. When you use an interface, you are telling the Typescript compiler that data you label with that interface will structurally resemble the interface. If you tell the compiler you are going to produce data with a specific shape, but the data you produced does not actually fit that shape, the compiler will throw an error. Another important feature of a contract is that once it is written and signed, it is sealed. The core contract itself cannot be changed. But it can be amended. The purpose of this article is to show some techniques for *amending* existing interfaces, such that they can be reused for data that is related to or closely resembles the interface.

Let's define a basic interface to use as an example throughout this article.

```
interface Question {
  content: string;
  answer: string;
  points: number;
}
```

Now, if I create a variable and give it a type question, it better have all three of those properties, with the correct types or I will get an error.

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

Now, if you add properties to `Question`, they will automatically get added to `MultipleChoiceQuestion`. This can be a bad thing (type 'gorilla banana problem' into Google), but in this particular case, since we *know* that `MultipleChoiceQuestion` will always be a form of `Question`, it's okay.

A major drawback to `extends` is that it results in tight-coupling between the inherited interface, `Question` in this case, and all the interfaces extending it. Imagine you wanted to add a property to `Question` that you didn't want on `MultipleChoiceQuestion` ... if you find yourself in this situation, just don't use `extends` for that case. It's no longer the appropriate tool.

Let's look at some other options.

### Intersection & Union types



Use extends

Create a generic type on your component that would look something like this:

@Component({
  selector: 'example'
  template: `<h1>example</h1>`
})
export class ExampleComponent<T extends UserData> {}
Now you can have data of type T which must have all of the UserData properties, then add whatever other properties you'd like.

You can also use extends to create a new interface for this specific case.

interface ExtendedUserData extends UserData {
 someOtherProperty: string
}
Use Intersection types

This may be close to the sort of "dynamic" behavior you're looking for. Say you have a property that needs to be an object with all the properties of UserData and an additional extraProperty.

You can type it like this.

fancyUserData: UserData & { extraProperty: string }
With intersection types you can add properties ad hoc.

So in short, no you can't really build dynamic types, and definitely not in the component's constructor. However, you can use the methods I mentioned above to extend and mold your interfaces on an as needed basis.