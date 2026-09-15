

`tsconfig.json`
```json
{
  // some other settings
  "compilerOptions": {
    // some other options
    "strictNullChecks": true,
    "strictPropertyInitialization": true,
    "noImplicitAny": true,
  }
}
```

- `strictNullChecks` - see [[ts.types#optionality]]
- `target` - 
- `module` - This is the version of Javascript that our code will get compiled to
  - ex. if we set `module` to `commonjs`, then the Typescript will be compiled into Javascript using `require` and `module.exports` syntax.
  - server-side project that uses Node.js should probably use CJS, while front-end applications should probably use ESM
- `target` - This setting changes which JS features are downleveled and which are left intact.
  - ex. `() => this` will be turned into an equivalent function expression if target is ES5 or lower.
  - `target` also changes the default value of `lib`.

# Resources
- [tsconfig options](https://www.typescriptlang.org/docs/handbook/compiler-options.html)
- [tsconfig bases](https://github.com/tsconfig/bases#centralized-recommendations-for-tsconfig-bases)
