
# Overview
The essential concepts in RxJS which solve async event management are:
- Observable: represents the idea of an invokable collection of future values or events.
- Observer
- Subscription: represents the execution of an Observable, is primarily useful for cancelling the execution.
- Operators: are pure functions that enable a functional programming style of dealing with collections with operations like map, filter, concat, flatMap, etc.
- Subject: is the equivalent to an EventEmitter, and the only way of multicasting a value or event to multiple Observers.
- Schedulers: are centralized dispatchers to control concurrency, allowing us to coordinate when computation happens on e.g. setTimeout or requestAnimationFrame or others.

RxJS introduces Observables, a new Push system for JavaScript. An Observable is a Producer of multiple values, "pushing" them to Observers (Consumers).
- A Function is a lazily evaluated computation that synchronously returns a single value on invocation.
- A [[generator|js.lang.functions.generator]] is a lazily evaluated computation that synchronously returns zero to (potentially) infinite values on iteration.
- A [[Promise|js.lang.promises]] is a computation that may (or may not) eventually return a single value.
- An [[Observable|general.patterns.behavioural.observable]] is a lazily evaluated computation that can synchronously or asynchronously return zero to (potentially) infinite values from the time it's invoked onwards.

*"Think of RxJS as Lodash for events."*

Because RxJS centers around this concept of function purity. Normally, we create impure functions which leave our state exposed, making it more prone to errors from other pieces of code.
- ex. if we have a variable `count` and increment it each time a button is clicked, there is nothing stopping some other piece of code mutating that value itself.
  - alternatively, with observables, we can just create an observable out of the event (`fromEvent`)

Marble diagram:
![](/assets/images/2022-06-02-10-41-07.png)

### Observers
An observer is a collection of 3 callbacks that knows how to listen to values (ie. types of notification) delivered by the Observable.
- those types of notification are: 
  1. the observer got the next value
  2. the observer got the last value
  3. the observer got an error

An observer may be *partial*

* * *

### Example: event listeners
Normally we register event listeners by finding an HTML element and calling `addEventListener` on it. With RxJS, we can create an observable that represents a stream of events.
- ex. we can attach an observable to a `<button />` element. We can then configure observers to `subscribe` to this observable, and will be notified any time the button is clicked:
```js
var button = document.querySelector('button');
Rx.Observable.fromEvent(button, 'click')
  .subscribe(() => console.log('Clicked!'));
```

# Questions
- what is meant by inner/outer observable
    - [source](https://academind.com/learn/javascript/callbacks-vs-promises-vs-rxjs-vs-async-awaits/)

# Resources
- [Operators](http://reactivex.io/documentation/operators.html)
- [thinking in nested streams](https://rangle.io/blog/thinking-in-nested-streams-with-rxjs/)
