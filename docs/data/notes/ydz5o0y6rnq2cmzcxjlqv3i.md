
### `jest.fn()`
Allows us to mock a function

Used when we want to mock a function, and don't really care about its original implementation (often just mocking the return value)
- returns a `spy` (which remembers each call that was made on it)
- creates a stub
- useful in removing dependencies to some backend (eg. server, API)

If we pass an argument, then it means we are passing a mock implementation:

```js
const queryMock = jest.fn(() => Promise.resolve({ data: 1 })
console.log(queryMock()) // Promise { pending }
```

### `jest.spyOn(object, methodName)`
Creates a mock function like `jest.fn`, but also tracks calls to the method, allowing us to spy on them
- this allows to ensure that the functions are called as expected (e.g. with correct arguments)

spies still call the original code, they aren’t complete mocks. They are called spies because they "spy" on the implementation code.

```ts
jest.spyOn(AutomationsDataSource.prototype, 'deleteAutomation').mockResolvedValue()
```

Or, we can add conditional logic:
```ts
jest.spyOn(AutomationsDataSource.prototype, 'getAutomation').mockImplementation((id) => {
  let automation: Automation
  switch (id) {
    case a1.id:
      automation = a1
      break
    case a2.id:
      automation = a2
      break
    default:
      automation = undefined
  }
  return Promise.resolve(automation)
})
```

We can also mock as many methods as we want:
```ts
jest.spyOn(AutomationsDataSource.prototype, 'getAutomation', 'deleteAutomation').mockImplementation((id) => {
// ...
```

### `jest.mock()`
`jest.mock` allows us to mock an entire module

If we call `jest.mock` with only 1 arg,
- the module we pass will be replaced simply with `jest.fn`

If we call `jest.mock` with 2 args,
- the module we pass will be replaced with the return value of the function provided (arg2).
    - in other words, if there is 2 args, `jest.mock` says "replace all occurrences of arg1 with arg2"

This code tells jest, "any time `calculateAge` is called, stick in 42 as its return value"
- we say "any time you import `calculateAge` into some other module, replace its value with `jest.fn(() => 42)`
```js
jest.mock('../calculateAge', () => {
  return jest.fn(() => 42);
});

// This runs the function specified as second argument to `jest.mock`.
const calculateAge = require('../calculateAge');
calculateAge(); // Will return '42';
```

#### Example
```ts
jest.mock('../components/box.jsx', () => 'Box')
```
- this is saying "for the component we are testing, take all instances of the `Box` component and replace them with the text `Box`"
- what's happening underlying is that since components are just functions, we are saying "replace our component function with the function `() => 'Box'`"
- we do this when we don't really care about all the props that come with box, but are just interested in the structure. `<Box />` doesn't really matter, since that isn't what we are testing.
- we do this basically when our proptypes fails, because we aren't supplying data in the way proptypes would expect (ex. passing to `Box` margin as `25px` rather than the expected `25`)

When we do `jest.mock('./my-file.js')`, it will turn all functions into Jest mock functions and all objects will have more properties on them like, `mockReturnValueOnce`

spec: Mocks get hoisted, so if there is a variable you define *after* the mock, you probably won't be able to use it

```js
jest.mock('./isValidCoupon', () => ({
    // isValidCoupon: async () => true,
    isValidCoupon: jest.fn().mockImplementation(async () => true),
}))
```

### Mocking individual functions
Imagine our implementation code file is made up of individually exported functions, and we want to mock one of them. We can use `jest.requireActual` to import the original implementation, but mock individual functions:
```ts
jest.mock('./automationVersions', () => {
  const origModule = jest.requireActual('./automationVersions')
  return {
    ...origModule,
    doesDraftExist: jest.fn(),
  }
})
```

Then, in our unit test we can mock the return value:
```ts
const mockedDoesDraftExist = jest.mocked(doesDraftExist)
mockedDoesDraftExist.mockReturnValue(true)
```
