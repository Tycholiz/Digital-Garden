
By default, Jest tests complete once they reach the end of their execution. Therefore, async tests will not work by default

## Callbacks
This code doesn't work, because `fetchData` returns before `callback`
```js
// Don't do this!
test('the data is peanut butter', () => {
  function callback(data) {
    expect(data).toBe('peanut butter')
  }

  fetchData(callback);
})
```

We can solve this with the `done` arg
```js
test('the data is peanut butter', done => {
  function callback(data) {
    try {
      expect(data).toBe('peanut butter')
      done()
    } catch (error) {
      done(error)
    }
  }

  fetchData(callback)
})
```
Now, Jest will wait until the `done` callback is called before finishing the test.
- if never called, it will fail with a timeout error, which is the desired way of handling this.
-  If we want to see in the test log why it failed, we have to wrap expect in a try block (as shown above)