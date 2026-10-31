
### Get elapsed time
```js
const start = Date.now()
doSomeExpensiveOperation()
const end = Date.now()
const timeElapsed = end - start
```

### Get Date-only (e.g. `2023-03-02`)
```js
new Date().toLocaleDateString()
```