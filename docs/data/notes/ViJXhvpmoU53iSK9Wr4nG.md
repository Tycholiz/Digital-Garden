
## Idempotency
In mathematics, the an idempotent function is one that will yield the same output, even as its output feeds back as input indefinitely, such as:
`Math.abs(Math.abs(Math.abs(Math.abs(-2))))`
`String( String( x ) )`
a way to test mathematical idempotency is to check if the output of a single function is equal to multiple calls of that function:
```js
currency( -3.1 ) == currency( currency( -3.1 ) )
// true
```
Wherever possible, restricting side effects to idempotent operations is much better than unrestricted updates.

The programming-oriented definition for idempotence is similar, but less formal. Instead of requiring `f(x) === f(f(x))`, this view of idempotence is just that `f(x);` results in the same program behavior (in other words, not just the output. *All* impacts of the function) as `f(x);` `f(x);`. In other words, the result of calling `f(x)` subsequent times after the first call doesn't change anything.
- Pure functions are idempotent in the programming sense

If you need to do side-effects, try to make it idempotent

Pure functions can reference `free variables` (ie. those that are not defined within the function scope), as long as those variables are sure to not change during the execution of the program (ie. they are constant)

An example of idempotency is in Postgres, when we say `delete function if exists`, rather than just `delete function`
- It is idempotent because running the command more than once has no additional effect than simply running it once.
