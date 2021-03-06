
# useEffect
- *The question is not “when does this effect run”, the question is “with which state does this effect synchronize with:”*
	- `useEffect(fn)` - all state
	- `useEffect(fn, [])` - no state
	- `useEffect(fn, [these, states])`
- if one of the variables in the dependency array changes, `useEffect` runs again. If the array is empty the hook doesn't run when updating the component at all, because it doesn't have to watch any variables.
- if you have no dependency array and are getting infinite loops, see if there are functions being defined each time in the component. It might be a simple fix to memoize them with `useCallback`
	- simply adding an empty dependency array is a bandaid solution and is not really addressing the root of the problem.
- By passing an empty array, we’re saying “only ever do this once”.
	- on rare occasions that’s ok, but most of the time you want something in there. The reason is that usually you want to synchronize with something in your code, not just perform the effect once

## Shortcoming of useEffect
- Compared to class component lifecycle methods, the shortcoming of `useEffect` is that we while we can set new state, we are unable to access current state (because of stale closure)
```js
useEffect(() => {
    const intervalId = setInterval(() => {
        setCount(count + 1)
    }, 1000)
    return () => clearInterval(intervalId)
}, [])
```
- in this example, `count` is always pointing to the previous reference.
- we can work around this shortcoming with refs. Essentially, because refs exist between renders, that value is never lost between mounts. We simply take the value from the ref, and update the state with that value:
```
useEffect(() => {
    const intervalId = setInterval(() => {
        countRef.current = countRef.current + 1
        setCount(countRef.current)
    }, 1000)
    return () => clearInterval(intervalId)
}, [])
```

`this` is mutable state, and the problem with mutable state is that it is always up to date. The good thing about hooks in react is that there is no `this` get retrieve values from. State always stays the same within a given render of a component.
