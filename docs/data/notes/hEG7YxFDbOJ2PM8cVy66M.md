
`useCallback` allows you to cache an instance of a function between renders.

The purpose of useCallback (and [[useMemo|react.lang.hooks.useMemo]]) is to prevent unnecessary re-renders and make your code more efficient.
- In the days of class components, we had a `render()` function. Anything placed inside would get re-calculated on every render, but we could also give the class methods (putting functions outside of `render()`). These methods would not get recalculated on each render, and would lead to performance gains. Since functional components naturally call everything (`render()` method is implicit), they inherently have a performance issue. `useCallback` and `useMemo` exist to alleviate this problem.
- You should consider using useCallback and/or useMemo hooks on the following situations:
1) Processing large amounts of data
2) Working with interactive graphs and charts
3) Implementing animations
4) Incorporating component lazy loading (useMemo specifically)

## `useCallback`
- useCallback returns a memoized callback.
- `useCallback` accepts a function, and returns the same instance of the function being passed instead of creating a new one when a component re-renders, which is the default behavior.
-  with this hook, a new function is only created when the variables passed to the dependency array change (kind of like `useEffect`). Otherwise, we use the same instance of that function between re-renders of the component (ie. different instances of the same component).
- Recall that in React, components will only re-render (and will ALWAYS re-render) when there are changes to either its state or changes to the props it receives. This means that not only will the JSX re-render, but the functions defined in the component will be called each time. But what if we want to prevent those functions from being called each time, and only call them when some values change?
```js
const additionResult = useCallback(add(firstVal, secondVal), [firstVal, secondVal])
```
- In this example, the `additionResult` doesn't necessarily get called each time the component re-renders. In fact, it only gets called if either `firstVal` or `secondVal` have changed between those 2 renders.

- [when to use](https://kentcdodds.com/blog/usememo-and-usecallback)
