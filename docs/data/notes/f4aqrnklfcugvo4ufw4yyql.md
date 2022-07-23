
`useMemo` allows you to cache a value between renders.
- the cache is local to the component position within the tree

It is similar to [[useCallback|react.lang.hooks.useCallback]], but instead of returning a function, it calls the function and gives us the return value of the function (again, only when one of the dependencies change)
- In other words, useMemo calls the passed function only when necessary and it returns a cached value on all the other renders.
- `useMemo` "caches" the value, while `useCallback` "caches" the function itself.

### Example
Imagine we had a function that we passed down to a child component.
```js
const getItems = () => {
	return [number, number + 1, number + 2]
}
```
[Fayez example](https://gist.github.com/fayezosaadi/8d54fe30d8bf855c23e6d7c13a31e346)
