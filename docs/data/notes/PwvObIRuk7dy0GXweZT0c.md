
- An observable is a function that returns a stream
- `$` is used to indicate that the variable in question is observable
- an observer is an object with `next`, `error` and `complete` methods
- Observables are inert (they just sit there until they are *subscribed* to), while observers stay active and listen for events from the producers

**epic** - a collection of observables

```js
export default function fetchTeams(action$) {
  // epic
  return action$.pipe(
    ofType('FLASHCARDS_TEAMS_GET'),
    switchMap(() =>
      queryGraphQL('flashcards__teams'),
    ),
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
1. Let’s assume the user types the letter "a" into our input (node is a variable that has query selected the html input element)
2. The Observable then reacts to this event, passing the value to the next observer
3. The value “a” is passed to `.map()`, which is subscribing to our initial observable
4. `.map()` returns a new Observable of event.target.value and calls `.next()` on it’s observer
5. The `.next()` call will invoke `.filter()`, which is subscribing to `.map()`, with the resulting value of the .map() call
6. .filter() will then return another Observable with the filtered results, calling `.next()` with the value if the length is 2 or above
7. We get the final value through our `.subscribe()` block

Each time a new Observable is returned, a new observer is hooked up to the previous Observable (allowing us to pass values along a stream of observers, which do something we've asked, then call `.next()`, then pass the result to the next observer.
- Basically, an operator returns a new Observable each time, allowing the stream to continue

## Higher-order Observables
- [[api|rxjs.op.api#higher-order-observables,1:#*]]

Observables normally emit scalar values, but sometimes we must handle Observables of Observables (so-called higher-order observables)
- We work with these by flattening, which means to convert it from a higher-order observable into a regular observable.

Higher-order observables have their own set of operators that can be used on them.

Since higher-order observables have an outer observable and 1+ inner observables, it is helpful to think of them in that way.

An output (e.g. from `tap(console.log)`) of `{_isScalar: false, _subscribe: ƒ}` would imply we are trying to work with a higher order observable as if it were a plain observable.
- this is similar to how when we forget to `await` a promise, we get `Promise<pending>`.

### Example: Search box
Imagine we have a search box, where each time the text changes, a request is sent to the server.
- the text changes are an observable (outer)
- each response is an observable (inner)