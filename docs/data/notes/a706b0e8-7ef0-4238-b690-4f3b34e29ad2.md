
### Destructuring
- With this technique we can pull out values from an array or object declaratively
```js
const arr = [1, 2, 3, 4]
const [ a, , b ] = arr
// a = 1
// b = 4
```
- when spreading, we are unpacking a variable one level deeper (into an array, object etc)
    - the `...` is unpacking, the `{}` is putting it into an object
    - ***ex.*** - if we had an array of objects, the unpacking by itself would result in us having an invalid javascript, since it would just be a group of objects with no "bucket" to house them (such as an array or object). This is why `{ ...arrayOfObjects }` will result in those "stray" objects having found a home in the surrounding `{}`
```js
newObj = {
    0: {},
    1: {},
    ...
}
```
```js
<Component a={obj.a}, b={obj.b} c={obj.c} /> === <Component {...obj} />
```
we can rename the object keys of a destructured object like so:
- an object is returned from calling `useQuery` with fields `loading` and `error`, which we rename.
```js
const { loading: queryLoading, error: queryError, data } = useQuery()
```

Destructuring creates a shallow copy

# UE Resources
[advanced destructuring](https://dmitripavlutin.com/javascript-object-destructuring/)
