---
layout: post
title:  "Think Before You Test: How to Write Effective Unit Tests Custom Events in Angular"
description: "A brief guide on unit testing custom events in Angular components."
date: 2018-03-22
---

Angular was built for testability. Powerful tools like dependency injection, the `TestBed` API, and out-of-the-box integration with Jasmine give us the power to test our Angular apps thoroughly and reliably. The catch is that learning these API's can take some time. Throw in a mix of Angular, Jasmine, and RxJS jargon and it can be a real uphill battle to feel comfortable testing the hairier parts of your application, which are the most important parts to test of course. In this post, I'll cover a couple different approaches you can take to testing custom events in Angular. If this is helpful or interesting to you, you can check out my [twitter page](https://twitter.com/_vincecampanale), where I share similar content.

## What is an `@Output` property? 

An `@Output` property is an Angular utility used to create custom events. An `@Output` is an [`EventEmitter`](https://angular.io/api/core/EventEmitter), meaning it has two methods: `emit` and `subscribe`. You probably won't need to `subscribe` to it directly, since Angular handles that with it's event binding syntax (e.g. `<div (someEvent)="onSomeEvent()"></div>`). The `emit` method allows you to notify the parent of an event and pass data up.

## What should a unit test for a custom event? 

When the component emitting the custom event (the child component), the unit test should target two things: 1) the `@Output` property's `emit` method is invoked when it should be and 2) the `emit` method is emitting the expected data.

When testing the component listening to the `@Output` (the parent / container component), the unit test should aim to check that the data emitted from the event goes to the right place (e.g. passed to the correct method).

## The component

The child component we'll be working with:

```
@Component({
  selector: 'counter',
  template: `
    <div>
      <button (click)="onClick()">1</button>
    </div>
  `
})
export class CounterComponent {
  @Output() change = new EventEmitter<number>();

  onClick() {
    this.change.emit(1);
  }
}
```

The `change` property is the `EventEmitter` we will be testing. 

We will use it in our `AppComponent` to increment a counter by the amount emitted:
```
@Component({
  selector: 'my-app',
  template: `
  <counter (change)="onChange($event)"></counter>
  `
})
export class AppComponent  {
  count = 0;

  onChange(event: number): void {
    this.count += event;
  }
}
```


## Testing the child

First, we'll do some set up:
```
describe('CounterComponent', () => {
  let fixture: ComponentFixture<CounterComponent>;
  let component: CounterComponent;
  let de: DebugElement;
  let button: ElementRef;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [CounterComponent]
    });
  });

  beforeEach(() => {
    fixture = TestBed.createComponent(CounterComponent);
    component = fixture.componentInstance;
    de = fixture.debugElement;
    button = de.query(By.css('button'));
  });
});

```

I won't go into the details of how this set up works, as it's outside the scope of this post. [Angular's testing tutorial](https://angular.io/guide/testing) is a great resource to learn more about it. What matters is that we can test everything we need to test using `component` and `button`.

Every custom `@Output` must get triggered by another event. Whether that event be a click in the DOM, a response from the server, a custom event on yet another nested child component, there must be a cause for the `emit` method to be called. The first step is to mock that cause and ensure the `EventEmitter` actually emits.

We know from the component code that a click event on the button should make the `onClick()` method run. The `change` property's `emit` method should be called when `onClick()` executes. We can get `onClick()` to execute in two ways: mock a `click` on the button, or just call `component.onClick()` directly.

Here is one of many ways to mock a `click` on the button:

```
button.nativeElement.click();
```

In order to detect when the `@Output` will emit, we can create a spy:
```
spyOn(component.change, 'emit');
```

Now you have everything you need to effectively test the `@Output`.

A unit test might look like this:
```
describe('change', () => {
  it('should emit when the button is clicked', () => {
    spyOn(component.change, 'emit');
    button.nativeElement.click();
    expect(component.change.emit).toHaveBeenCalled();
  });
});
```

And that's it. Now, let's target goal #2: ensure the `@Output` is emitting the expected data to the parent. 

Using `toHaveBeenCalledWith()`, we can kill two birds with one stone:

```
describe('change', () => {
  it('should emit when the button is clicked', () => {
    spyOn(component.change, 'emit');
    button.nativeElement.click();
    expect(component.change.emit).toHaveBeenCalledWith(1);
  });
});
```

Now, in one unit test, you are ensuring that the `emit` method is being called when it should be and that it is emitting the correct data. There are a couple of other ways to accomplish this, which are worth mentioning.

I think it's safe to say that Angular has `click` events down, so we don't need to worry about that not working as expected. Because of this, it is safe to call the `onClick()` method directly, instead of mocking a click on the button.

```
describe('change', () => {
  it('should emit when the button is clicked', () => {
    spyOn(component.change, 'emit');
    component.onClick();
    expect(component.change.emit).toHaveBeenCalledWith(1);
  });
});
```

This is a bit easier because we don't have to worry about querying the `DebugElement` or mocking click events, we just call the method  we care about and trust Angular to handle the rest.

A final approach to testing the `EventEmitter` is to actually subscribe to it and trigger the event, making your assertion in the subscribe block.

```
describe('change', () => {
  it('should emit when the button is clicked', () => {
    component.change.subscribe(next => {
      expect(next).toEqual(1);
    });
    
    component.onClick(); // or button.nativeElement.click()
  });
});
```

I don't recommend this approach for a couple of reasons:  
   1. It's weird. Typically, a unit test makes its assertion(s) at the *end*. This approach breaks that pattern and will cause future developers to have to look sideways and squint to understand how the test works. Unit tests should be easy to read and understand.   
   2. Order of statements matters. If you call `component.onClick()` before subscribing to the `change` emitter, you will not actually get into the subscribe block and make the assertion. This is made even worse by the fact that your test will pass! A faulty, passing test is worse than no test at all.
  

# Testing the parent

We can take three approaches to testing the behavior of the `EventEmitter` from the perspective of the parent (the component listening for the event):  
  1. Invoke the `@Output` property's `emit` method (since the `EventEmitter` is a public property)  
  2. Dig in to the counter's `DebugElement` and simulate a click on the button
  3. Call the function directly (trust that Angular will work)
  
Here's what the set up looks like:

```
describe('AppComponent', () => {
  let fixture: ComponentFixture<AppComponent>;
  let component: AppComponent;
  let de: DebugElement;

  beforeEach(() => {
    TestBed.configureTestingModule({
      declarations: [AppComponent, CounterComponent]
    });
  });

  beforeEach(() => {
    fixture = TestBed.createComponent(AppComponent);
    component = fixture.componentInstance;
    de = fixture.debugElement;
  });
});
```

In order to invoke the `@Output` property's `emit` method, we had to declare the component with the `@Output` in the testing module.

Now, we can use the `DebugElement` for the `AppComponent` to get the counter component:

```
describe('onChange', () => { 
  it('should be called with whatever the counter change event emits', () => {
    spyOn(component, 'onChange');
    const counter = de.query(By.directive(CounterComponent));
    const cmp = counter.componentInstance;
    cmp.change.emit(1);
    expect(component.onChange).toHaveBeenCalledWith(1);
  });
});
```

In the above unit test, we spy on the `onChange` method (the method that should be called when `change` emits). Then, we query for the counter component fixture based on it's directive class and get the component itself through the `componentInstance` property. *Now*, we have access to the `change` property and can tell it to `emit` a value of `1`. In order to test that we are handling the event correctly, we'll just check that the `onChange` spy gets called with the value that the `change` event emitted. This is overkill, but not nearly as overkill as the next test.

```
describe('onChange', () => {
  it('should be called with whatever the counter change event emits', () => {
    spyOn(component, 'onChange');
    const counter = de.query(By.directive(CounterComponent));
    const button = counter.query(By.css('button'));
    button.nativeElement.click();
    expect(component.onChange).toHaveBeenCalledWith(1); 
  });
});
```

Now we are querying the fixture of the child element for the actual, physical button and dispatching a `click` event to the button. This `click` event will fire off the chain reaction which should eventually lead to our `AppComponent`'s `onChange` method being called with the value emitted from the `change` event. But wait, let's check in with what we're actually testing here. A unit test should be responsible for one *unit* of functionality. The test we just wrote is testing 1) that the button's click works, 2) that Angular's handling of the click event works, 3) that our `onClick` method in the `CounterComponent` gets called with the correct data and makes the appropriate call the `change` property's `emit` method, 4) that Angular's handling of the `change` event works, 5) that our `onChange` method works. That's not a unit test. 

Now that you've seen all the crazy stuff you *can* do with this powerful set of testing tools, you will be relieved to see what you actually *need* to do.

```
describe('onChange', () => {
  it('should increment the count by the amount provided', () => {
    component.count = 2;
    component.onChange(2);
    expect(component.count).toEqual(4);
  });
});
```

The only thing that *actually* needs to be tested on this end is the `onChange` method itself. That's the only logic we wrote. Everything else is handled by Angular. Feel free to double check [the `EventEmitter` tests](https://github.com/angular/angular/blob/master/packages/core/test/event_emitter_spec.ts) if you're skeptical.

# Takeaways

Tests are good. We have a lot of powerful tools at our disposal for testing in Angular so it's easy to make sure our components work as they should. Finally, it's important to understand the difference between what we *can* test and what actually needs to be tested.



