
A thunk is a pure function that returns an impure function 

## The pattern of providing dispatch to a helper function
- imagine we wanted to dispatch 2 actions that normally occur together, such as an action to show a notification, and an action to hide it 5 seconds later. The simplest approach is to dispatch an action to show it, then dispatch an action 5 seconds later to hide it. But what if we want to use this combination of 2 actions elsewhere in the app? What we can do is create an external function that accepts `dispatch` as the first arg, and the payload that goes to both dispatches as the rest of the args. 
	- Not only does this give us code reuse, but it also allows us to have better control over *that* action, and only that action. In our first naive method, we have no idea if the notification related to the action being dispatched 5 seconds later is associated with the notification related to the action dispatched instantly. For instance, if a notification is trigged and then milliseconds later another is triggered, we might end up in a race condition where our app doesn't know how to match up the `show` and `hide` actions with the correct notification. 

```js
//Naive method
this.props.dispatch({ type: 'SHOW_NOTIFICATION', text: 'You logged in.' })
setTimeout(() => {
  this.props.dispatch({ type: 'HIDE_NOTIFICATION' })
}, 5000)

// external function to increase code reuse
let nextNotificationId = 0
export function showNotificationWithTimeout(dispatch, text) {
  // Assigning IDs to notifications lets reducer ignore HIDE_NOTIFICATION
  // for the notification that is not currently visible.
  const id = nextNotificationId++
  dispatch(showNotification(id, text))

  setTimeout(() => {
    dispatch(hideNotification(id))
  }, 5000)
}
```

## What are thunks
- A thunk is a middleware that gives `dispatch` the ability to accept a function. Normally dispatch can only accept an action (usually dispatch accepts the invocation of an action creator, which returns an action). With Thunk middleware (mw being something that gives enhanced functionality), `dispatch` gains the ability to accept functions.  
	- When a function is dispatched, Thunk MW will recognize that and pass `dispatch` as the first argument, allowing us to dispatch any action we want within the function (ie within the thunk)
- The result is a function that is very similar to the external helper function we made before. There are 2 key differences: 
	- our new function doesn't accept `dispatch` as the first prop
	- our new function *returns* a function of its own, and *that* function accepts `dispatch` as it's first argument
```js
let nextNotificationId = 0
export function showNotificationWithTimeout(text) {
  return function (dispatch) {
    const id = nextNotificationId++
    dispatch(showNotification(id, text))

    setTimeout(() => {
      dispatch(hideNotification(id))
    }, 5000)
  }
}
```

## Motivation for Thunks
- the above solution works but it presents 2 issues:
	- we have to pass `dispatch` around everywhere. This means that where we need to call that helper function, we need to have access to `dispatch`, meaning we need to create a container for that component. 
	- We have to mentally keep track which redux `actions` are just regular action creators, and which are actually more like action helpers (above example. since they do not return an action, they are not action creators)

## Using thunks
without the Thunk MW, we could execute this function like so:
```
showNotificationWithTimeout('You just logged in.')(this.props.dispatch)
```
but since we are using the MW, any time a function is dispatched instead of an action object, thunk takes over and calls that function with `dispatch` as its first argument, giving the function access to `dispatch`
- therefore, we just do this 
```
this.props.dispatch(showNotificationWithTimeout('You just logged in.'))
```

With thunks, dispatching an asynchronous action (really, a series of actions) looks no different than dispatching a single action synchronously to the component. Which is good because components shouldn’t care whether something happens synchronously or asynchronously. We just abstracted that away. The component only knows about the thunk, and the thunk deals with the async logic

Thunks also receive as the second arg the application store. This is useful for if we want to bail out of an API call 
- ex. user has notifications turned off:
```js
let nextNotificationId = 0
export function showNotificationWithTimeout(text) {
  return function (dispatch, getState) {
    // Unlike in a regular action creator, we can exit early in a thunk
    // Redux doesn’t care about its return value (or lack of it)
    if (!getState().areNotificationsEnabled) {
      return
    }

    const id = nextNotificationId++
    dispatch(showNotification(id, text))

    setTimeout(() => {
      dispatch(hideNotification(id))
    }, 5000)
  }
}
```
- note:  If you use `getState()` only to conditionally dispatch different actions, consider putting the business logic into the reducers instead.

Thunks may also return promises. In fact, Redux doesn’t care what you return from a thunk, but it gives you its return value from `dispatch()`
- This is why you can return a Promise from a thunk and wait for it to complete by calling `dispatch(someThunkReturningPromise()).then(...)`
- You may also split complex thunk action creators into several smaller thunk action creators. The dispatch method provided by thunks can accept thunks itself, so you can apply the pattern recursively. Again, this works best with Promises because you can implement asynchronous control flow on top of that.

## When thunks aren't enough, and to reach for Sagas
For some apps, you may find yourself in a situation where your asynchronous control flow requirements are too complex to be expressed with thunks.
- ex. retrying failed requests, reauthorization flow with tokens, or a step-by-step onboarding
* * *
[source](https://stackoverflow.com/questions/35411423/how-to-dispatch-a-redux-action-with-a-timeout/35415559#35415559)
