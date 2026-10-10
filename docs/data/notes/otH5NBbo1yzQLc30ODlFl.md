
## Operator precedence
Operators with higher precedence become the operands of operators with lower precedence.
- ex. `true || false && false` evaluates to `true`, because the `false && false` is evaluated first, which returns `false`, which is then used as the operand for the original expression: `true || false`, which evaluates to `true`

Use parentheses (`()`) to alter precedence