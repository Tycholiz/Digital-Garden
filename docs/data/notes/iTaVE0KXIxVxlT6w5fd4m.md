
Only declarations are hoisted, not initializations
- ex. `const name` is hoisted, `name = "Kyle"` is not.
- therefore if you access a variable before it's declared, the value is always `undefined`

`var`-declared variables are hoisted, meaning you can refer to the variable anywhere in its scope, even if its declaration isn't reached yet. You can see var declarations as being "lifted" to the top of its function or global scope.