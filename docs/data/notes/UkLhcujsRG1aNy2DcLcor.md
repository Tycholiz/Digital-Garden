
Modules import one another using a module loader. At runtime the module loader is responsible for locating and executing all dependencies of a module before executing it. Well-known module loaders used in JavaScript are Node.js’s loader for CommonJS modules and the RequireJS loader for AMD modules in Web applications.

In TypeScript, just as in ECMAScript 2015, any file containing a top-level import or export is considered a module. Conversely, a file without any top-level import or export declarations is treated as a script whose contents are available in the global scope (and therefore to modules as well).

Top-level code is executed only once during module initialization. 

# ES Modules (ES6 Import)
Since imports aren't a part of the Javascript standard yet, imports are just a specification— it is up to the implementation (Babel, Typescript, Metro Bundler(?)) to carry out the operation of linking modules.

ESM is for Browsers (though we can use transpilers like Babel to enable its use in Node)
- ex. Typescript's `tsconfig.json` file has a setting `target` which determined which version of Javascript the code will be transpiled to. Therefore, if we use `target`

ESM is tree-shakable

ESM are required as they are needed, rather than there being a bundle created beforehand (as with CommonJS)

If the `type` field of [[package.json|js.node.package-json]] is `module`, then `.js` files will be interpreted as ESModules.
- Even if `"type": "commonjs"`, we can still use ESModules by giving our files a `.mjs` extension.

when you tell the JS engine to run a module, it has to behave as though these four steps are happening:
1. Parsing: The implementation reads the source code of the module and checks for syntax errors.
2. Loading: The implementation loads all imported modules (recursively). (This is the part that isn’t standardized yet)
3. Linking: For each newly loaded module, the implementation creates a module scope and fills it with all the bindings declared in that module, including things imported from other modules.
	- This is the part where if you try to import {cake} from "paleo", but the “paleo” module doesn’t actually export anything named cake, you’ll get an error. And that’s too bad, because you were so close to actually running some JS code. And having cake!
4. Run time: Finally, the implementation runs the statements in the body of each newly-loaded module. By this time, import processing is already finished, so when execution reaches a line of code where there’s an import declaration… nothing happens!
- when we import a module like `import 'firebase/storage'` (ie. not importing any bindings), it means we are executing the module `firebase/storage`, but not bothering to assign the default export to a variable. In fact, perhaps the module doesn't even export any bindings.
	- This implies we are doing side-effects.

- When you `import *`, what’s imported is a *module namespace object*. The properties of this object are the module’s exports:
```js
[Module] {
  default: '[Function]', // the default export
  first: 'Kyle' // named export `first`
  last: 'Tycholiz', // named export `last`
}
```
- if we wanted to import the named exports, we could `import { first } from _____`. spec: also, we could import the default by `import default from _____` (or `import { default }`?)

- `import _ from "lodash"` is an alias for `import { default as _ } from "lodash"`
* * *
Imported ES6 modules are executed either asynchronously or synchronously, depending on the module loader (ie. the implementation) we use. Therefore, to be safe we must assume async. However, all imports are executed prior to the script doing the importing. This makes ES6 modules different from Node.js modules or `<script>` tags without the `async` attribute

### Importing without name
ex. `import './bootstrap'`
- this will execute the target module (ie. run the module's code), without importing anything. It will not affect the scope of the active module
	- There may be side-effects, such as declaring global variables.
- This method of importing is described as "importing a module for its side-effects only"

### Aggregating modules (Re-exporting)
- We can import modules and immediately export them again by aggregating the import and export commands:
```
export * from './atoms'
```
- If any name exported by “atoms” happened to collide with the other exports, that would be an error, so use export * with care.

Unlike a real import, this doesn’t add the re-exported bindings to your scope, meaning we can't use the exports from "atoms" within that file.

# CommonJS Imports
This is the Node.js way of handling imports (as of 2022)

Modules are copied

Imports are synchronous

Tree-shaking doesn't work with CommonJS (since this type of thing typically doesn't matter with server code)

If the `type` field of [[package.json|js.node.package-json]] is `common-js` (or is empty), then `.js` files will be interpreted as CommonJS.
- Even if `"type": "module"` (indicating ESModules), we can still use CommonJS by giving our files a `.cjs` extension.

## module.exports
- `module.exports` is an object that is included in every `.js` file in a Node application.
	- `module` represents the current module
	- `exports` is an objects that will be exposed as a module
	- Therefore whatever we assign to `module.exports` is exposed as a module.
- like `exports` below, `module.exports` can also be extended by including more properties/methods on the object.

before a module's code is actually executed, Node will wrap it in a function that looks something like this:
```js
(function(exports, require, module, __filename, __dirname) {
	// Module code actually lives in here
});
```
This gives us the benefit of:
- scoped variables, rather than global variables.
- ability to use `module` and `exports` objects.
- ability to reference the module's absolute filename and directory path with `__filename` and `__dirname`

## exports
- `exports` is an object that we can attach properties and methods to.
- when we import a module, we must then call the same property/method:
```js
// dependency
exports.name = 'Kyle'
exports.phone = '5555774834'

// dependent
const person = require('./information')

console.log(person.name) // Kyle
```

# CommonJS vs ES6 Modules
Under the hood, we need something like Babel to convert from ES6 modules to CommonJS.
- [explanation](https://stackoverflow.com/questions/40294870/module-exports-vs-export-default-in-node-js-and-es6)

# Circular Dependencies
- Not always a problem, but they introduce tight coupling.
	- These kinds of modules are harder to understand and reuse, as doing so might cause a ripple effect where a local change to one module has global effects.
	- As such, it might indicate lack of a larger context or proper architecture, since a good architecture imposes uni-directional flow between modules and entire layers.

using `const` over `function` while defining functions prevents function hoisting within a single module and ensures the absence of circular dependencies within that module.

Circular dependencies with Function calls would not cause problems when the cycle is asynchronous, meaning that directly referenced functions are not called immediately.
- ex. Cycle of function calls when one continues chain through a DOM event listener being async, i.e. waiting for user click.

# Dynamic Imports
`import(module)` loads the module and returns a promise that resolves into a module object that contains all its exports. `import` can be called from anywhere in the code.

example
```js
import(modulePath)
  .then(obj => <module object>)
  .catch(err => <loading error, e.g. if no such module>)
```
or
```js
// 📁 say.js
export function hi() {
  alert(`Hello`);
}

export function bye() {
  alert(`Bye`);
}
```
then
```js
let {hi, bye} = await import('./say.js');

hi();
bye();
```
Note: Although `import()` looks like a function call, it is specified as syntax that just happens to use parentheses (similar to super()). That means that import doesn’t inherit from Function.prototype so you cannot `call` or `apply` it.

Dynamic means code that can be executed at runtime. Static means it occurs before compilation and before runtime. 
- ex. dynamic imports. in this case, dynamic means that we can decide the path at runtime, or we can decide which module to import at runtime

# UE Resources
- https://v8.dev/features/modules
- [Differences between ESM and CommonJS](https://nodejs.org/api/esm.html#esm_differences_between_es_modules_and_commonjs)


re