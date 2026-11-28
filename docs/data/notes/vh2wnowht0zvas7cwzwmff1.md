
`toThrow` executes a function passed in expect, and verifies that it throws an error.

In this case, you're not passing expect a function. You're executing a function that throws an error. toThrow is never executed, and even if it were, it would throw an error, telling you that it should receive a function, but received something else instead.

`expect(someFunctionThatThrows())` is essentially the same as `expect(throw new Error())`.

## Resources
https://jestjs.io/docs/expect#tothrowerror
