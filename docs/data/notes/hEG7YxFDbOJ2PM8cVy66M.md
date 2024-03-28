
`useCallback` returns a memoized callback.
- it accepts a function, and returns the same instance of the function being passed instead of creating a new one when a component re-renders, which is the default behavior.

`useCallback` allows you to cache an instance of a function between renders.
- by default, all variables (including functions) defined in a component body will be regenerated on each rendering of the component. `useCallback` allows us to say "I know that this function body will not change unless *this* prop (or state) changes. Therefore, I want to opt-out of the automatic regeneration of the function *unless* that prop changes."

The purpose of `useCallback` is to prevent unnecessarily re-defining functions between component renders, making our code more efficient.
- [[useMemo|react.lang.hooks.useMemo]] "caches" the value, while `useCallback` "caches" the function itself.
- In the days of class components, we had a `render()` function. Anything placed inside would get re-calculated on every render, but we also had the option of defining functions as class methods (by putting functions outside of `render()`). These methods would not get recalculated on each render, and would lead to performance gains. Since functional components naturally call everything (since `render()` method is implicit), they inherently have a performance issue. `useCallback` and `useMemo` exist to alleviate this problem.

You should consider using useCallback and/or useMemo hooks on the following situations:
1. Processing large amounts of data
2. Working with interactive graphs and charts
3. Implementing animations
4. Incorporating component lazy loading (`useMemo` specifically)


```js
const additionResult = useCallback(add(firstVal, secondVal), [firstVal, secondVal])
```
- In this example, the `additionResult` function only gets re-defined if either `firstVal` or `secondVal` have changed between those 2 renders.

### Example
Imagine we had a function that we passed down to a child component.
```js
const getItems = () => {
	return [number, number + 1, number + 2]
}
```

This can get expensive. Instead we can memoize the callback:
```js
const getItems = useCallback(() => {
	return [number, number + 1, number + 2]
}, [number])
```

Now, the `getItems` function:
```js
() => {
	return [number, number + 1, number + 2]
}
```

Only gets redefined when the `number` prop changes.

## Additional reading
- [when to use](https://kentcdodds.com/blog/usememo-and-usecallback)
