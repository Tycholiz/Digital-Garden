
Mocking is a technique to isolate test subjects by replacing dependencies with objects that you can control and inspect. It allows us to test the links between code by erasing the actual implementation.
- we can introspect how many times a mocked function/method was called (along with the args passed), and assert that it matches what we'd have expected to happen.
- The goal for mocking is to replace something we don’t control with something we do, so it’s important that what we replace it with has all the features we need.

Mocks encourage testing based on behavior verification
- The mock is not so interested in the return values of functions. It’s more interested in what function were called, with what arguments, when, and how often.
  - therefore a mock is always a spy.

if an object returns or throws based on data passed in then it's a fake, not a mock.

We mock dependencies because a) we don't care about it in this unit test, and b) we want a predictable result from the external module, so that we can test the main unit under those different conditions
- ex. if we are mocking a call to Postgres, we want to subject our main unit to testing that will allow it to yield expected results when it is hammered with different inputs provided by an external module. If the postgres dependency returns a populated array, how does the main unit respond? how about if the array is empty? how about if it returns 404?

Imagine we have a function that does some computation, then sticks the result in a new file. Since we are unit testing, we aren't testing whether or not the file gets created— that can be safely assumed (since we will be unit testing that separately). Instead, we can safely mock the creation of the file.
- If we didn't mock this, we'd have to clean up after ourselves each time we run a test. It would result in a slower testing suite.

### Examples
#### forEach
If we were writing a unit test for `forEach`, we would just mock the callback.
```js
function forEach(items, callback) {
  for (let index = 0; index < items.length; index++) {
    callback(items[index]);
  }
}
```

Imagine we had a mock function:
```js
const mockCallback = jest.fn(x => 42 + x);
forEach([0, 1], mockCallback);
```

There are a few things we care about when it comes to unit testing this function:
- was the callback called twice?
- was the first argument of the first call `0`?
- was the return value of the first call `42` (42 + 0)?

#### Difference between mock and stub
A mock is like a stub but the test will also verify that the object under test calls the mock as expected
ex. You can stub a database by implementing a simple in-memory structure for storing records. The object under test can then read and write records to the database stub to allow it to execute the test. This could test some behavior of the object not related to the database and the database stub would be included just to let the test run. If you instead want to verify that the object under test writes some specific data to the database you will have to mock the database. Your test would then incorporate assertions about what was written to the database mock.
