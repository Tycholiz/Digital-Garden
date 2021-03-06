
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

- ***ex.*** `jest.mock('7g-components/box/box.jsx', () => 'Box');`
    - this is saying "for the component we are testing, take all instances of `Box` and replace them with the text `Box`"
    - what's happening underlying is that since components are just functions, we are saying "replace our component function with the function `() => 'Box'`"
    - we do this when we don't really care about all the props that come with box, but are just interested in the structure. `<Box />` doesn't really matter, since that isn't what we are testing.
    - we do this basically when our proptypes fails, because we aren't supplying data in the way proptypes would expect (ex. passing to `Box` margin as `25px` rather than the expected `25`)

When we do `jest.mock('./my-file.js')`, it will turn all functions into Jest mock functions and all objects will have more properties on them like, `mockReturnValueOnce`

spec: Mocks get hoisted, so if there is a variable you define *after* the mock, you probably won't be able to use it

```js
jest.mock('./isValidCoupon', () => {
  return {
    // isValidCoupon: async () => true,
    isValidCoupon: jest.fn().mockImplementation(async () => true),
  }
})
```
