
Setting state only changes it for the *next* [[render|react.architecture#rendering]]
- ex. if we have a button that sets the state multiple times, using the existing state as a dependency, we would get unexpected behaviour, and the `number` state would only increment once per click. If the initial state of `number` is `0`, then that is the value that is used each time `setNumber` is called:
```jsx
<button onClick={() => {
    setNumber(number + 1);
    setNumber(number + 1);
    setNumber(number + 1);
}}>
    +3
</button>
```

Phrased another way, on any given render, the state (and props) are *always* the same.

If you would like to update the same state variable multiple times before the next render, instead of passing the next state value like `setNumber(number + 1)`, you can pass a function that calculates the next state based on the previous one in the queue, like `setNumber(n => n + 1)`
- this is a way to tell React to “do something with the state value” instead of just replacing it.
- this is called an **updater function**, and when you pass it to a state setter:
    1. React queues this function to be processed after all the other code in the event handler has run.
    2. During the next render, React goes through the queue and gives you the final updated state.
- technically, `setNumber(6)` is identical to `setNumber(x => 6)`

When something can be calculated from the existing props or state, don’t put it in state. Instead, calculate it during rendering.

### Avoiding State Paradoxes
ex. `isTyping` and `isSubmitting` can’t both be true, so instead of having these as distinct states, replace with with a `status` state that can be `typing`, `submitting`, or `success`.

### Batching
As of React 18, React will wait until all code in the event handlers have run before processing your state updates.
- therefore, re-rendering doesn't occur until the event handler has finished execution
- anal: a waiter doesn't return to the kitchen with an order after each guest gives their order— Instead, they let you finish your order, let you make changes to it, and even take orders from other people at the table, before notifying the kitchen.

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

### Forcing a Re-render
Imagine we had a chat application with 2 components: `<ContactList />` and `<Chat />`. The `<Chat />` component takes `contact` as a prop, which is an object with the shape: `{ name: 'Taylor', email: 'taylor@mail.com' }`.
- by default, the `<Chat />` component won't re-render, even if the object changes. We can however, force the component to re-render by passing it the `key` prop like this: `<Chat key={to.email} contact={to} />`. That way, when the `to.email` value changes, the component will be re-rendered.
- [original example](https://react.dev/learn/managing-state#preserving-and-resetting-state)