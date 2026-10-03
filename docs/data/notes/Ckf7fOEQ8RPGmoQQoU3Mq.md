
Note: as of 02/2022, this is still only stage two, so polyfills are needed

Useful any time we want to do something like this:
```js
const result = double(double(increment(double(2))))
```

With pipeline operator, we can do this:
```js
2 |> double |> increment |> double |> double
```

