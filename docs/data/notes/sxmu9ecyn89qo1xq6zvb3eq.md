
The reason why we mock is that we want to replace something we don't control  with something that we do control.
- "don't control" can mean a third-party module, or even just a function/class that we wrote that we are using in the current unit we are testing.

Mock functions provide benefits:
- allows us to test the links between different units by erasing implementation details of a function
- allows us to peek into the mock (via the `.mock` property), giving us valuable information on it like, 
    - it was called or not 
    - which parameters were passed to it
    - what the return value was
- allows us to change the implementation of the original function
- allows us to set return values

We can create mock functions in 2 different ways:
1. creating a mock function to use in test code
  - `jest.mock` - mock an entire module (e.g. axios)
  - `jest.fn`
  - `jest.spyOn`
2. writing a [manual mock](https://jestjs.io/docs/manual-mocks) to override a module dependency
  - involves making a `__mocks__` directory

Mock functions return undefined by default, but we can inject return values into it:
```js
const myMock = jest.fn();
console.log(myMock());
// > undefined

myMock.mockReturnValueOnce(10).mockReturnValueOnce('x').mockReturnValue(true);

console.log(myMock(), myMock(), myMock(), myMock());
// > 10, 'x', true, true
```

In order to mock properly, Jest needs `jest.mock('moduleName')` to be in the same scope as the import statement (ie. top level).

### Remaking `jest.fn`
- [source](https://www.pluralsight.com/guides/how-does-jest.fn()-work)
```js
// 1. The mock function factory
function fn(implementation) {
  // 2. The mock function
  const mockFn = function(...args) {
    // 4. Store the arguments used
    mockFn.mock.calls.push(args);
    return implementation(...args); // call the implementation
  };
  // 3. Mock state
  mockFn.mock = {
    calls: []
  };
  return mockFn;
}
```

### Axios example
Imagine we are testing a module (`Users`) which internally calls `axios.get`: 
```js
class Users {
  static all() {
    return axios.get('/users.json').then(resp => resp.data);
  }
}
```

Of course, we don't want our `Users.test.js` unit test to actually make a GET request. Therefore, we need to mock it with `jest.mock`

## UE Resources
https://www.pluralsight.com/guides/how-does-jest.fn()-work