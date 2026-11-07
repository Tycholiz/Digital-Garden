
`redux-observable` is a redux middlware allowing us to `map` and `filter` an `action$` (ie. an action as an observable) with RxJS operators.
- While javascript `map` and `reduce` allows us to transform arrays, these versions allow us to transform *streams of actions*

## Epic
- ***def*** - a function that takes in a stream of actions, and returns a modified stream of actions.
  - receive an observable as input
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

