
Prefer `useReducer` over `useState` when either:
1. The state is complex, where it has multiple sub-values (ie. an object)
2. the next state depends on the previous state

`useReducer` also lets you optimize performance for components that trigger deep updates because you can pass dispatch down instead of callbacks.
