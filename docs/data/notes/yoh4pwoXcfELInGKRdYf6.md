
# Overview
The following table shows what role observables fulfill. When we want to get a single value for a synchronous action, we set the value to a variable. When we want to get a single value for an async action, we use promises. When we want to get the value for an array synchronously, we use an array. Finally, when we want to get the value of an array of values asynchronously, we use observables.

|          |sync    |async     |
|----------|--------|----------|
|single    |variable|promise   |
|collection|array   |observable|

Observables are like arrays because they represent a collection of events but are also like promises as they’re asynchronous: each event in the collection arrives at some indeterminate point in the future.
- This is distinct from a collection of promises (like `Promise.all`) as an observable can handle an arbitrary number of events, and a promise can only track one thing.

An observable can be used to model clicks of a button. It represents all the clicks that will happen over the application’s lifetime, but the clicks will happen at some point in the future that we can’t predict.
```js
let myObs$ = clicksOnButton(myButton);
```

## "Mapping + Flattening" Operators
- ***flatten*** - subscribing inside a subscribe
- All work mostly in same manner
    - They map some value to an observable (you are the one in charge of returning an observable value from them, they just map it)
    - They flatten the observable you return ( they just subscribe to it)
    - They decide about what to do before / after they flatten (“Flattening Strategy”)
### mergeMap - the slacker operator
- simply keep subscribing to every new observable that we return from the map
- Other than mapping + flattening the observable, it does nothing else.
- ex. Imagine Netflix shows up-to-date ratings for each movie, retrieved from IMDB's API. We can `mergeMap` the movie into an http request to IMDB to get this data and enhance our UI.
### switchMap - the "latest and greatest" operator
- unsubscribe from the last mapped observable
- ex. Imagine we are typing in Google and the autocomplete box shows up. Of course, these suggestions change with each key press. If we use `switchMap`, each previous request will be cancelled if a new one happens. If we'd used `mergeMap`, a request for each keystroke would be made
### concatMap - the "wait in line" operator
- queue up the observables one after the other, and play their events in that order (i.e. subscribe to the next Observable in the queue only when the previous one is completed).
- Similar to `mergeMap`, except order matters
- ex. top 10 list
### exhaustMap - the "do not disturb" operator
- ex. login button - since we don't want multiple clicks to be registered, we want want to disable mapping while the first http request is on the go, ensuring that we never call the server while the current request is running.


# Redux-Observable (library)
- ***def*** - a redux middlware allowing us to `map` and `filter` actions with RxJS operators.
    - While javascript `map` and `reduce` allows us to transform arrays, these versions allow us to transform *streams of actions*
## Epic
- ***def*** - a function that takes in a stream of actions, and returns a modified stream of actions.
    - receive `variable$` as input
- epics can be thought of as a description of what additional actions redux-observable should dispatch
- epics are analogous to sagas from redux-saga

example:
```js
const pingEpic = action$ => action$.pipe(
  ofType('PING'), //equivalent to filter(action => action.type === 'PING')
  mapTo({ type: 'PONG' })
);

// later...
dispatch({ type: 'PING' });
```
- pingEpic will listen for actions of type PING and map them to a new action, PONG. This example is functionally equivalent to doing this:
```js
dispatch({ type: 'PING' });
dispatch({ type: 'PONG' });
```
- Epics run alongside the normal Redux dispatch channel, after the reducers have already received them. When you map an action to another one, you are not preventing the original action from reaching the reducers; that action has already been through them

## Operators
- `ofType` - filter by a specific type of action

# Stream
- ***def*** - a sequence of data elements made available over time
- Can be thought of as items on a conveyor belt being processed one at a time, rather than in large batches
- Can also be seen as a sequence of ongoing events ordered in time
    - ex. number of button clicks in 1 second
        - All the clicks will be grouped together as a stream
- The stream is the subject which is being observed

* * *
### Promise vs Observable
- Promises handle a single event (ie. the failure or success of an asyn operation).
- An observable is a function that returns a stream, and we can pass in zero or more events to it. The callback provided to the observable will be called for each event.
	- Observables are preferred over promises because they don't care how many events you have, while promises require 1, and only 1.
- Observable libraries provide methods to help interact with the emitted (returned?) value(s). For instance, we can use `map` to transform each value's output
- a Promise is eager, whereas an Observable is lazy
- If you wanted to subscribe to the reactive way of programming, then you could just "observable all the things"

Additionally...
- Observables are cancellable.

# Questions
- what is meant by inner/outer observable
    - [source](https://academind.com/learn/javascript/callbacks-vs-promises-vs-rxjs-vs-async-awaits/)

# Resources
- [Operators](http://reactivex.io/documentation/operators.html)
- [thinking in nested streams](https://rangle.io/blog/thinking-in-nested-streams-with-rxjs/)
