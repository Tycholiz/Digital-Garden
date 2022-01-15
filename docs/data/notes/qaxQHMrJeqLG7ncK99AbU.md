
Allows us to mock a function

Used we want to mock a fn, and don't really care about its original implementation (often just mocking the return value)
- returns a `spy` (which remembers each call that was made on it)
- creates a stub
- useful in removing dependencies to some backend (eg. server, API)

If we pass an argument, then it means we are passing a mock implementation:

```js
const queryMock = jest.fn(() => Promise.resolve({ data: 1 })
console.log(queryMock()) // Promise { pending }
``
