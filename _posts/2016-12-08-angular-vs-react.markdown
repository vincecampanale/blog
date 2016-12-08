---
layout: post
title:  "Angular vs. React"
date:   2016-12-08
categories: jekyll update
---
Last night, I attended a Google Developer Group DC ([@gdevdc](https://twitter.com/gdevdc)) sponsored "TechDebate" hosted at the Washington D.C. campus of [1776](http://www.1776.vc/).
The debate, called "Battle of the Application Frameworks: Angular vs. React," was way more organized and civilized than I expected. I was imagining (hoping for?) a panel of nerds heatedly arguing about JavaScript frameworks: spit flying, glasses fogging, Star Wars-related name-calling. Alas, this event was formatted and moderated as a real debate. It was a very informative hour.

{% include image.html name="techdebate.jpg" caption="The crowd eagerly awaits the showdown." %}

Here's a summary of the debate and the main points of each team (key points bolded):

### Initial Speeches

#### Angular Constructive (3 minutes)
* Angular is:
  * consistently proven
  * future-proof
  * backed by a strong and unified codebase
* With React, you have to herd all these disparate libraries together (big mess)
* Angular is better for real-world problems because
  * it is easier to maintain
  * it has a standardized style guide
  * separation of concerns is baked in
* Angular has evolved to version 2 after learning lots of lessons from mistakes in version 1 and taking notes on what works for React

#### React Constructive (3 minutes)
* React, at its core, is just JavaScript. It's simple. If you know JavaScript, you know React.
  * Angular, on the other hand, is it's own thing
* React is so simple and un-opinionated it's basically not even a framework
* React brings back *JavaScript* development as opposed to *framework* development
* Angular sucks because it only supports evergreen browsers
* React allows you to use your own modules and build your own components -- **it's much more flexible**
* Google doesn't even use Angular for their own big apps (Gmail), while Facebook uses React for lots of key features
* Angular is big, bulky, and slow
* React has an amazing community

#### Angular Rebuttal (5 minutes)
* Let's talk frameworks (Angular) vs. libraries (React)
  * Frameworks give you everything you need (btw Angular will now let you use NativeScript and build Native apps)
  * An amalgamation of libraries is a big mess
* While it may not be used for Gmail *yet*, there *are* big names using Angular - weather.com, theguardian.com, etc.
* Angular *does* support old browsers
* Tree shaking, lazy loading, and ahead-of-time compiling are all means developers can take to get rid of extra code and make an Angular app rather lightweight
* Too many options with React -- too much complexity. This makes it harder to find help when something breaks
* Angular has a living style guide and Angular 2 has great documentation, so it's very easy to find help
* Angular is a **stable, complete solution** that lets you focus on building your application rather than picking libraries and managing dependencies

#### React Rebuttal (5 minutes)
* Anyone who knows JavaScript can write React, Angular is not even JavaScript anymore. It's TypeScript.
* Learning React lets you keep up your JavaScript skills; learning Angular stagnates you and turns you into an "Angular developer" rather than a "JavaScript developer"
* Angular is a good one-stop shop, but do you really *need* a one-stop shop?
* Stuff like tree shaking and ahead-of-time compiling was taken from React -- React is ahead of the curve, Angular is behind
* NativeScript is not used widely, ReactNative is very well used
* *Appeals to audience:* "Remember digest cycle?"
* Angular style guide only exists because they have bad documentation

### Cross Examinations

#### Angular cross-examines React
Q. What do you do on-boarding a new team member for your React application? How do you get them acquainted with the code base?

A. React Storybook. You can share your custom components and codebase with your team.

#### React cross-examines Angular
Q. How does Angular fare when building applications that are accessed in third-world countries and places with low bandwidth?

A. Tools like tree shaking, ahead-of-time compiling, using imports, and lazy loading allow you to reduce the size of your Angular application significantly. TypeScript compiles to ES6 or ES5 so it will work most places.

Q. How do you debug an Angular app?

A. Angular CLI is great for debugging.

### Judge's Questions
Q. How does the developer experience compare? Why is your framework better for the developer?

* React Answer: JavaScript is ubiquitous. When you are using React, you're just writing JavaScript. For Angular, you have to learn Angular, TypeScript, etc.
* Angular Answer: Yes, Angular does require you to learn new stuff, but it is very intuitive and easy to pick up. Once you get it, it's very useful. Additionally, TypeScript is quite pleasant to code in and helps with debugging by preventing mistakes up front.

Q. How is corporate backing good/bad for your respective frameworks?

* React Answer: Facebook is just backing JavaScript, because React is just JavaScript. Frameworks come and go, but it is the language that remains. React just handles view and does it well.
* Angular Answer: A major challenge with JavaScript is wading through the countless variations, modules, packages, plugins, and frameworks. What we need is a standard. Angular is progress toward that standard. It is backed by a great company (Google) and they want to push web standards in order to make the web more accessible and easier to develop for.

Q. Maintainability/testability?

* React Answer: React gives you the flexibility to do whatever testing you want. Mocha, Karma, etc.
* Angular Answer: Angular basically has built in unit testing. You can do end-to-end testing and UI testing (both described in tutorials). TypeScript helps a lot with maintainability. Dependency injection allows you to test your parts individually.

Q. What are you most happy your community ripped from the other one? What do you hope is stolen next?

* React Answer: We can't think of what React would want from Angular. React is looking to the future.
* Angular Answer: Angular moved entirely towards components, which is nice.

### Final Question from Audience
What are you most excited about for the future?
* React Answer:
  * WebGL. React + WebGL = Future.
* Angular Answer:
  * More progress towards standards. Angular gives you a comprehensive toolset that allows you to focus on content, rather than wasting time picking and maintaining your individual tools.

#### And the winner is...
React won the debate by a hair. The judges tied 2-2 (a lesson in choosing the number of judges for the next debate) and the deciding factor was the team who had the "Most Valuable Debater" as decided by the audience.

## My Take On The Whole Thing
So here's what I deduced from this back-and-forth. First, I took everything said with a grain of salt. There are lots of other frameworks and libraries out there. React and Angular are hands-down two of the best options. While each side tried to make the other sound terrible for the sake of the debate, there is no doubt that both frameworks are effective for developing efficient, high-quality, scalable applications. Both frameworks are viable options.

I boiled each side's core argument down into a little analogy: React is the capable teenager, exploring the unknown, always seeking the next cool thing. Angular is the wizened elder, striving for unity and direction in a chaotic world.

React is flexible, dynamic, and embraces crafting unique solutions to unique problems. It truly embodies the Wild West of JavaScript, allowing people to mix and match libraries, build and customize their own modules, and basically get as creative as they want. Angular is sturdy, comprehensive, and powerful -- a highly opinionated one-stop shop. It reflects a movement towards more standardized, reliable web technology which is a breath of fresh air for the many experiencing JavaScript "tool fatigue."

All in all, it was a great time and I'm looking forward to the next #TechDebate!
