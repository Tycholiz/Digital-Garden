
Jest runs our code as plain Javscript. That means that any part of our code that isn't plain js (e.g. jsx, TypeScript types) needs to be transformed into plain js.
- A transformer is just a synchronous function that transforms source files into plain js.
see [transform configuration option](https://jestjs.io/docs/configuration#transform-objectstring-pathtotransformer--pathtotransformer-object)

## Implementations
[ts-jest: a Jest transformer with source map support allowing us to use Jest to test TypeScript code](https://kulshekhar.github.io/ts-jest/)
