

### `map`
analogous to `Array.map` in that it applies a given *project function* on each value of a collection. 
- while `Array.map` accepts an array and returns an array, `Observable.map` accepts an observable and returns an observable.

### `pipe`
Takes in observable(s) as input and returns another observable
- `pipe` is immutable, so previous observables remain unmodified.

By using `pipe` we decouple the streaming operations (map, filter, reduce...) from the core functionality (subscribing, piping).

Even if there is only one observable, we should still use `pipe`

[[see: pipeable operators|rxjs.op#pipeable-operators,1:#*]]

### `from`
Turn an array, promise, or iterable into an observable.

Similar to `of()`, except `from()` emits the items that are inside the argument it receives, while `of()` emits the argument as a whole.

### `fromEvent`
Creates an observable that emits certain events (e.g. click events) coming from a given event target.
```js
const source = fromEvent(button, "click")
const subscribe = source.subscribe(val => console.log(val))
```

### `of`
`of()` returns an Observable which immediately emits whatever values are supplied to `of()` as parameters, then completes.
- This is better than returning static values, as it allows you to write subscribers that can handle the Observable type (which works both synchronously and asynchronously), even before implementing your async process.

Emit variable amount of values in a sequence and then emits a complete notification.

`of` is useful for maintaining the Observable data type before implementing an asynchronous interaction 
- ex. an http request to an API

Contrary to what the docs might suggest, `of()` is not deprecated. Calling `.of()` on an observable is deprecated, so we must change the way we use it:
```js
// before
Observable.of(1,2,3).map(x => 2 * x);

// after
of(1,2,3).pipe(map(x => 2 * x));
```

### `pluck`
`pluck` is used when we just need to pass single field value to the subscription instead of sending entire JSON object.

### `tap`
`tap` is a place to perform side-effects. 
- Typically we use it with `map` or `mergeMap`, so the question becomes "why don't we just perform our side-effects in those methods?"
  - Performing side-effects here makes those `map` methods impure, limiting ourselves. For instance, it would prevent us from being able to memoize.

`tap` returns an observable that is identical to the source with the only difference being that if there was an error, it will be emitted from the returned observable.

Conveniently, we can also use `tap` for debugging
- can place a `tap(console.log)` anywhere in your observable pipe, log out the notifications as they are emitted by the source returned by the previous operation.

```js
of(Math.random()).pipe(
  tap((value) => console.log(value)),
  map(n => n > 0.5 ? 'big' : 'small')
).subscribe(console.log);
```

### `combineLatest`
This operator combines multiple Observables to create an Observable whose values are calculated from the latest values of each of its input Observables.
- Whenever any input Observable emits a value, it computes a formula using the latest values from all the inputs, then emits the output of that formula

## Higher-order observables
- ***flatten*** - subscribing inside a subscribe
- All work mostly in same manner
  - They map some value to an observable (you are the one in charge of returning an observable value from them, they just map it)
  - They flatten the observable you return ( they just subscribe to it)
  - They decide about what to do before / after they flatten (“Flattening Strategy”)

### `concatAll`
subscribes to each "inner" Observable that comes out of the "outer" Observable, and copies all the emitted values until that Observable completes, and goes on to the next one. 
- All of the values are in that way concatenated

### `mergeMap` (`map` + `mergeAll`) - the slacker operator
- simply keep subscribing to every new observable that we return from the map
- Other than mapping + flattening the observable, it does nothing else.
- ex. Imagine Netflix shows up-to-date ratings for each movie, retrieved from IMDB's API. We can `mergeMap` the movie into an http request to IMDB to get this data and enhance our UI.
- Maps each value to an Observable, then flattens all of these inner Observables using `mergeAll`.

### `switchMap` (`map` + `switch`) - the "latest and greatest" operator
- unsubscribe from the last mapped observable
- ex. Imagine we are typing in Google and the autocomplete box shows up. Of course, these suggestions change with each key press. If we use `switchMap`, each previous request will be cancelled if a new one happens. If we'd used `mergeMap`, a request for each keystroke would be made
- `switchMap` projects each source value to an Observable which is merged in the output Observable, emitting values only from the most recently projected Observable
- ie. it maps each value to an Observable, then flattens all of these Observables in the only output Observable.

### `concatMap` (`map` + `concatAll`) - the "wait in line" operator
- queue up the observables one after the other, and play their events in that order (i.e. subscribe to the next Observable in the queue only when the previous one is completed).
- Similar to `mergeMap`, except order matters
  - `concatMap` is `mergeMap` with a concurrency of 1.
- ex. top 10 list

### `exhaustMap` - the "do not disturb" operator
- waits for the current inner observable to complete (exhaust) so that the next item would be turned into an inner observable (items emmitted while inner observable is running are ignored). ExhaustMap is very similar to concatMap, but drops some values:
- `concatMap` - will take everything, inner observable n has to finish before n+1 will start
- `exhaustMap` - will ignore all items which *would* produce new inner observables, if there is an inner observable running

- ex. login button - since we don't want multiple clicks to be registered, we want want to disable mapping while the first http request is on the go, ensuring that we never call the server while the current request is running.

## UE Resources
https://betterprogramming.pub/how-to-create-observables-in-rxjs-aa3bf79b05e0