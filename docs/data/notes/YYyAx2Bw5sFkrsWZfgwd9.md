
## What is it?
- a saga is a daemon that lets us define long-running processes that take [[actions|redux.actions]] as they come, and transform or perform requests before outputting actions. 
- This moves the logic from action creators into sagas

A saga is a separate thread in the app that's just for side-effects
- while thunks utilize callbacks, a saga thread can be started, paused (`yield`) and cancelled by dispatching actions within [[generator functions|js.lang.functions.generator]]

In synchronous Redux, a dispatched action is assigned to a [[reducer|redux.reducers]]. In Async redux, a dispatched action is assigned to a saga. 
- The saga does its side effect (e.g. `resourceListReadRequest`), and takes the returned data, and dispatches another action (e.g. `resourceListReadSuccess`) which is then picked up by the reducer.

### Reconciling generator functions and Saga
- Imagine a saga as a thread that constantly calls `next()` and tries to execute the `yield` lines as soon as it can
- spec: like a promise, it will wait on that value to "return" before it calls `next()` and goes to the next `yield`
- In Sagas, we are "yielding" to the redux-saga middleware
	- The MW suspends the saga until the yielded side effect resolves. At this point, the MW calls `next()`
- When we say `yield call(___)`, we are only describing what we want to happen. We aren't describing the actual outcome. In this sense, it is declarative.

## Types of Saga
### Worker Saga
A worker saga is a saga that performs side effects and dispatches other actions asynchronously.

### Watcher Saga
A watcher saga is a saga that listens for dispatched actions and calls worker sagas in response.

### Root Saga
A root saga is a saga that runs all watcher sagas in parallel

## Example process
1. An action `resourceListReadRequest` is dispatched somewhere in the code
2. A *Watcher Saga* that is designed to listen for `resourceListReadRequest` picks up on the fact that it was dispatched, and notifys the *Worker Saga*

## API Reference
### Effects
*def* - an object containing instructions to be fulfilled by middleware. When the MW retrieves an effect yielded by a saga (ie. when a saga executes put, call, take etc), the saga pauses until the effect is done.
- `call` - call the fn (1st arg) with args (rest args)
- `put` - dispatch action
- `take` - block execution of the saga until the provided action is dispatched
	- therefore, this is used in a watcher saga
- `fork` - useful when a saga needs to start a non-blocking task
	- Non-blocking means: the caller starts the task and continues executing without waiting for it to complete
	- Situations:
		1. grouping sagas by logical domain
		2. keeping a reference to a task in order to be able to cancel/join it
- `takeEvery` - each time a particular action is dispatched, spawn a saga
	- `takeEvery` uses `take` and `fork` under the hood
- `takeLatest` - spawn a saga only for the latest dispatched action of a given type
	- ex.  imagine a user is mashing the login button. with Thunk, an API call would be made with each button press. with redux-saga, we get to just take the latest one and ignore the rest

# UE Resources
[redux-saga primer](https://flaviocopes.com/redux-saga/)
- [also](https://medium.com/appsflyer/dont-call-me-i-ll-call-you-side-effects-management-with-redux-saga-part-2-cd16f6bcdbcd)
