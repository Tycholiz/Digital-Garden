
In Javascript we often reach for `||` when we want to get the RH value if the LH value is falsy
- ex.
```js
const valA = nullValue ?? "default for A";
```

This way of doing it works most of the time, but if our business logic considers falsy values like `0` or `''` as legitimate values, then our program will have unexpected behavior. Using `??` gives us more predictable results, and should be used most of the time instead of `||`

### Assigning a default value to a variable
In the past, when one wanted to assign a default value to a variable, a common pattern was to use the logical OR operator (`||`):
```js
let foo;

//  foo is never assigned any value so it is still undefined
let someDummyText = foo || 'Hello!';
```

However, this is kind of hacky when we consider what we are doing. `||` is a boolean logical opertor (ie. it performs "math" on booleans). In order to do boolean math on 2 variables, it has to make sure both values are booleans. If they are not, they have to be coerced (Of course, JS is notorious for its sometimes unpredictable coercion behaviour).
- With boolean logical operators, the only two values that are considered are truthy values and falsy values. This might work most of the time, but sometimes our business logic dictates that we would expect `0` to be a truthy value. If this is the case, then

Example:

Below, `quantityOfApples` will not be `0` as we want. Rather, the default value of `10` is being used, because `0` is considered falsy in the eyes of the boolean logical operator.
```js
const count = 0

const quantityOfApples = count || 10 // 10 — unexpected!
const actualQuantityOfApples = count ?? 10 // 0 — totally expected
```

### Misc
- It is not possible to combine both the AND (`&&`) and OR operators (`||`) directly with `??`
	- To accomplish this, we need to use parentheses (`(null || undefined) ?? "foo";`)
