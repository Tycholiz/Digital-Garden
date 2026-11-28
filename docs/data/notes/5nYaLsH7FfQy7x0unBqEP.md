
***subscription*** - an object that represents the execution of an *observable*

Much like a promise, we need to unwrap our observable to access the values it contains. The observable unwrapping method is called subscribe. The function passed into subscribe is called every time the observable emits a value (in this case, a message is logged to the console anytime the button is clicked).

```js
let myObs$ = clicksOnButton(myButton);
myObs$
.subscribe(clickEvent => console.log('The button was clicked!'));
```

One thing to note here is that observables under RxJS are lazy. This means that if there’s no subscribe call on `myObs$`, no click event handler is created. Observables only run when they know someone’s listening to the data they’re emitting.

The function passed into subscribe is called every time the click event happens(the observable emits a value)

- `.create` accepts a subscribe function
    - `subscribe` accepts an *observer argument*
```js
// This first part is the observable, which emits things
const myObservable = Observable.create(function subscribe(observer) {
    observer.next('hey!') //this is emitting a value
})

// To grab the value, we define an observer
// (x is the observer)
const observer = myObservable.subscribe((x) {
    console.log(x) // hey!
})
```
