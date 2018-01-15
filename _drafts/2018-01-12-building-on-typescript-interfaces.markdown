---
layout: post
title:  "Can I build a dynamic interface with Typescript?"
description: "Exploring options for making Typescript interfaces more flexible."
date: 2018-01-12
---

No, you cannot dynamically change / create an interface as it is just a static value, used for structural type checking by the Typescript compiler.
However, you can get pretty creative with an interface and chances are you can mold it on the fly to fit your use-case.

### Interfaces are like contracts

A good analogy for a Typescript interface is a contract. An important part of a contract is that it requires the signer to follow a specific set of guidelines and if those guidelines are not followed, there are repercussions. When you use an interface, you are telling the Typescript compiler that whatever data you produce will structurally resemble that interface. If you tell the compiler you are going to produce data with a specific shape, but the data you produce does not actually fit that shape, the compiler will throw an error. Another important feature of a contract is that once it is written and signed, it is sealed. The core contract itself cannot be changed. But it can be amended. The purpose of this article is to show some techniques for *amending* existing interfaces, such that they can be reused for data that is related to or closely resembles the interface.

### Use `extends`

###



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