
Useful any time we want to do something like this:
```js
const result = double(double(increment(double(2))))
```

With pipeline operator, we can do this:
```js
2 |> double |> increment |> double |> double
```

Note: as of 06/2020, this is still only stage one, so polyfills are needed
