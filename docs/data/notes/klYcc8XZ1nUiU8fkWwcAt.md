
A variable may belong to one of the following scopes:
- Global scope: The default scope for all code running in script mode.
- Module scope: The scope for code running in module mode.
- Function scope: The scope created with a function.
- Block scope: The scope created with a pair of curly braces (a block).
    - only applicable to variables initialized with `let` or `const`

Lexical scope is scope that is determined by the JS engine before any code has even been executed (ie. it only looks at the source code— known as [lexing time](https://en.wikipedia.org/wiki/Lexical_analysis))
- when you see lexical, think "static"
- lexical scoping is analogous to prototypical inheritance, in a sense that the engine will walk up the chain to find a variable, if one is not found in the local scope.

blocks within if statements, else statements, while statements, and so on should ideally be one line long. If that is unachievable, then it should be striven for to have all code at a single level of indentation.
