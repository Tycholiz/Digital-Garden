
With automated batching in React 18, multiple state updates will always be batched.

The problem is that when we update multiple pieces of state in sequence, React will re-render the component. We need a way to tell React "hey, don't bother re-rendering until I've completed my whole sequence of state updates."

When we implement a click handler, we are implementing the same benefits that batching gives us. Only a single re-render would be triggered from the following click handler:
```js
function handleClick() {
  setIsFetching(false);
  setError(null);
  setFormStatus('success')
}
```

However, what if we wanted to change the 3 pieces of state *not* in response to an event handler? Imagine we want the state to be updated when a fetch resolves?