
- ***Observable*** - A data structure representing the idea of an invokable collection of future values or events
	- in other words, they are a representation of any set of values over any amount of time
- Observables allow us to compartmentalize our eventing flows, encapsulating each action in a single function
- Observables are lazy Push collections of multiple values
- An observable is a function that returns a stream
- An observable is a function that takes an observer and returns a cancellation function
- `$` is used to indicate that the variable in question is observable
- variables that reference a stream are denoted with `$` (ex. `action$`)
    - ex. *observable*
    - an observer is an object with `next`, `error` and `complete` methods
- Observables are inert (they just sit there until they are *subscribed* to), while observers stay active and listen for events from the producers
- The problem is this: we can consider an array where we already know all the values as eager, and an array that receives values (ie. increases length) at a set interval (1s) as lazy. Normally, we perform data processing in an eager way, since the data is processed immediately as it's received, which is instant. What if we are in a position where the array grows over time? It would be beneficial if we could call a method on an array value as it enters the array. In this sense, we need to subscribe to the array to execute a method when the array grows.

observables are conceptually similar to a fancy event emitter.

- There are 2 sides to an observable: producer and consumer
    - **Producer** - adds to the array
        - ex. button clicks add that click event to the array
    - **Consumer** - calls the function on the new array item
        - ex. calls `console.log` in response to the new click event

- An `Observer` subscribes (ie. is consumer) to an `Observable`
    - an observer is a collection of callbacks

Observables are lazy Push collections of multiple values. They fill the missing spot in the following table:

|      | Single   | Multiple   |
|------|----------|------------|
| Pull | Function | Iterator   |
| Push | Promise  | Observable |
- They *push* with `.next()`


## Characteristics
- They are time-independent (ie. lazy)
- They are mostly used in asynchronous data streams, like web sockets or multiple concurrent api calls

- Just as promises abstract time away from our concern for a single asynchronous operation, observables abstract time away from a set of data (eg. array)
    - ex. Observable can be set up to listen for mouseclicks, by pushing onto an array each time the mouse is clicked. The fact that the *subscribed* function exists means that it will fire whenever the array increases in size.


- If you combine the functionality of an Observer and an Observable, you get a Subject

- each operator on an Observable returns a new Observable, meaning they are chainable (this is known as a *stream*)

- A map(..) on an array runs its mapping function once for each value currently in the array, putting all the mapped values in the outcome array. A map(..) on an Observable runs its mapping function once for each value, whenever it comes in, and pushes all the mapped values to the output Observable.

**epic** - a collection of observables

```js
export default function fetchTeams(action$) {
    // epic
    return action$.pipe(
        //operator
        ofType('FLASHCARDS_TEAMS_GET'),
        //operator
        switchMap(() =>
            queryGraphQL('flashcards__teams'),
        ),
        //operator
        map(resp => { operator
            const flattenedData = flattenGraphqlNode(resp).data.teams;
            return getTeamsSuccess(flattenedData);
        }),
        catchError(err => {
            getError(err);
        }),
    );
}
```

## Using observables
- An `observable` is a function that takes in an `observer` (an object with `next`, `error`, and `complete`) and returns cancellation logic (i.e. unsubscribe).
- `.next()` is called when the observable produces values
- When an observer subscribes to an observable, the observer will keep receiving values until one of 2 things happens:
    - there are no more values to be sent (in which case `.complete()` is called)
    - the observer calls `.unsubscribe()` on the observer
- fromEvent will turn an event into an observable
    - `fromEvent(<event to be listened to>, <eventName>)`
```js
const input$ = Rx.Observable.fromEvent(node, 'input')
  .map(event => event.target.value)
  .filter(value => value.length >= 2)
  .subscribe(value => {
    // use the `value`
  });
```
Here are the steps of this sequence:
1. Let’s assume the user types the letter “a” into our input (node is a variable that has query selected the html input element)
2. The Observable then reacts to this event, passing the value to the next observer
3. The value “a” is passed to `.map()`, which is subscribing to our initial observable
4. `.map()` returns a new Observable of event.target.value and calls `.next()` on it’s observer
5. The `.next()` call will invoke `.filter()`, which is subscribing to `.map()`, with the resulting value of the .map() call
6. .filter() will then return another Observable with the filtered results, calling `.next()` with the value if the length is 2 or above
7. We get the final value through our `.subscribe()` block

- Each time a new Observable is returned, a new observer is hooked up to the previous Observable (allowing us to pass values along a stream of observers, which do something we've asked, then call `.next()`, then pass the result to the next observer.
    - Basically, an operator returns a new Observable each time, allowing the stream to continue
