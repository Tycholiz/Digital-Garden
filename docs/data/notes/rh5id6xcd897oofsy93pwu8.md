
## Logical AND (`&&`) - Logical Conjunction
If one of the operands is falsy, then that first falsy operand will be returned, and the rest of the operands will never be executed (therefore, it is a short-circuit operator)
- ex. `3 > 0 && -2 > 0` <-- returns `false`
- ex. `"" && "foo"` <-- returns `""`

If all of the operands are truthy, then the last operand will be returned
- ex. `3 > 0 && 6 > 0` <-- returns `true`

The value that is returned can always be converted to a boolean primitive by using the double `NOT` operator (`!!`)
- ex. `!!"" && "foo"` <-- returns `false`

The `AND` operator has a higher precedence than the `OR` operator, meaning the && operator is executed before the || operator

## Logical OR (`||`) - Logical Disjunction
If one or more of the operands is truthy, then that first truthy operand will be returned, and the rest of the operands will never be executed (therefore, it is a short-circuit operator)
- if all operands are falsy, then the last operand is returned

The value that is returned can always be converted to a boolean primitive by using the double `NOT` operator (`!!`)
- ex. `"" || !!"foo"` <-- returns `true`