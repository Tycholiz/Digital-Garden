
# Selectors 
If actions are the entry points, selectors are the exit.
- After dispatching an action, the application can use selectors to get the resulting data from the store after reducers and sagas have done something with it.
### useSelector
- equivalent to the purpose of `mapStateToProps`, which is provide easy access to the particular parts of the store that you want. 
	- when a component has multiple instances, the selector needs to be defined inside the component, so that each instance gets its own selector instance.

## Reselect
- Will memoize the value that is returned by the selected value
    - Imagine there is a list of questions in the redux store, and we want to retrieve all questions that have an even-numbered `id`. We could write a reselect selector that gets this array, performs a `filter` on it, memoizes (caches) that function, then return it to us. Then, next time we call that selector, the value is already available to us, rather than having to perform that `filter` operation on the array all over again.
- If we are just getting data without modification (ex. getting `store.title`), then reselect is not necessary. Only when there is expensive computation is reselct beneficial.
- `createSelector` will take in 1+ selectors (functions), call them, and passes their return values (the selected part of the store) as arguments to the final arg (an anonymous transform function) of `createSelector`. That function will accept 1 parameter for every selector that came before it. 
	- If one of the input selectors has changed since the last call, the transform function will be called. Otherwise, it will just return the memoized value.
    - this function is a thunk (returns a function)
    - ***ex.***
    ```
    export const getFeedbackForYou = createSelector(
        getByRecipient,
        (byRecipientState) => byRecipientState.myFeedback,
    );
    ```
    - here, `createSelector` is being called and returns a function that takes state as the argument (since that's what thunks do).
    - conceptually, we can therefore replace `createSelector(...)` with `(state) => 

* * *

- `mapStateToProps` is really just a type of selector when you think about it.
	- specifically, it is the selector that (potentially) uses other selectors to get data from the store. It is meant to clump them together for easy access to components that get rendered by the container 
- `state` refers to the data that currently exists in the store. `store` refers to the instance
