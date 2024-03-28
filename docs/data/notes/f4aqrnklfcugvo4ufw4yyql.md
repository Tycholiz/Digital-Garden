
`useMemo` allows you to memoize (cache) a value between renders of a component.
- By default, defining a variable inside a component means that it will be redefined and recalculated every time the component renders. `useMemo` allows us to say "ok, I know that only one of the props of this component really matters to the calculation of this variable, so only recalculate it when that one changes. All other prop changes shouldn't cause it to re-render. Therefore, in the event that none of the dependencies change, just return me the cached value.
- the cache is local to the component position within the tree
- this idea of "remembering" the value is in a way conceptually similar to a ref, in that the value will exist between renders. The difference is that while refs exist between *all* renders of a component, a variable defined by `useMemo` will exist only between the renders where none of the dependencies change.

Naturally, we should only use `useMemo` with pure functions. That makes sense, since impure functions by definition have dependencies that are different each time.

React will only cache one version of a memoized value. 
- ex. imagine we had a variable defined with `useMemo` that is dependent on `value`. The first render, `value` is `kyle`. Then `value` changes to `kyle t`, so the component re-renders and the variable is calculated again. Finally, `value` changes back to `kyle`, so the variable is recalculated again. Even though the value is the same as the first render, it won't be retrieved from the cache.

`const MyComponent = memo(() => null)` is an anonymous component and will appear as unnamed in React Devtools.

### Relation to `useCallback`
Like [[useCallback|react.lang.hooks.useCallback]], the purpose of `useMemo` is to prevent unnecessary variable re-calculations and make your code more efficient.

It is similar to `useCallback`, but instead of returning a function, it calls the function and gives us the return value of the function
- In other words, `useMemo` calls the passed function only when necessary and it returns a cached value on all the other renders.
- `useMemo` "caches" the value, while `useCallback` "caches" the function itself.

### Checking how expensive a calculation actually is
If the overall logged time adds up to a significant amount (say, 1ms or more), it might make sense to memoize that calculation. 
- Keep in mind that your machine is probably faster than your users’ so it’s a good idea to test the performance with an artificial slowdown. 
    - ex. Chrome offers a CPU Throttling option for this.

```js
console.time('filter array');
const visibleTodos = useMemo(() => {
  return getFilteredTodos(todos, filter); // Skipped if todos and filter haven't changed
}, [todos, filter]);
console.timeEnd('filter array');
```