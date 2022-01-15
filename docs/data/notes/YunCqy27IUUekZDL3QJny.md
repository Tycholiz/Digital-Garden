
TypeScript is a superset of JavaScript, meaning you can write and use JavaScript libraries from within TypeScript. In such situations how should TypeScript handle the lack of Type information in JavaScript? We can either:
- Accept the lack of types from the JS file
- use Declaration files, which give us an ad-hoc way to specify the shape of things in Javascript.

`.d.ts` files are analogous to C++ header files.

### Incorporating type declarations from third parties
When we want to use Javascript 3rd party libraries in our TS project, we can often find declaration modules as packages
- for example, `@types/lodash` is a module that gives us types out of the box with Lodash.
- these declaration files get saved in `node_modules/@types/`

When a type declaration is included in the `@types` directory, the types are automatically available for us to use in our project, as the TS compiler finds these types and makes them available during compilation time.

[DefinitelyTyped](http://definitelytyped.org/) is a project that hosts declaration files for popular JS libraries.

### Config
"typeRoots" and "types" are the two properties of the tsconfig.json file that can be used to configure the behavior of the type declaration resolution.

### Example
Imagine we had a Javascript file in a Typescript project:
```js
// main.ts
let ajala

ajala = {
 name: "Ajala the traveller",
 age: 12,
 getName: function() {
     return this.name;
   }
};

ajala.lol()
```
When we compile this file with `tsc main.ts`, the compilation would be successful, and `main.js` is outputted.

If we then run `node main.js`, we will get a runtime error that `ajala.lol is not a function`. Since no type information was available for it, Typescript could not have warned us about this at compile time.

To fix this, we can create a file `main.d.ts`:
```ts
declare module "MyTypes" {
	export interface Person {
		name: string;
		age: number;
		getName(): string;
	}
}
```

And then add a triple-slash directive and type annotation in `main.ts`
- note: this triple-slash directive is possible, but not recommended. Instead, just install npm type declaration modules (below)
```ts
/// <reference path="Main.d.ts" />

import * as MyTypes from "MyTypes"

let ajala: MyTypes.Person

ajala = {
	name: "Ajala the traveller",
	age: 12,
	getName: function() {
		return this.name;
	}
};

ajala.lol();
```
With the declaration file provided and included via the triple-slash directive, the TS compiler now has information about the shape of `Person`, and will throw us an error at compile time.

### How to type a third party module
1. create a `@types` folder at same level as `tsconfig.json`
2. add a `.d.ts` file using the format `[nameOfTheThirdPartyModule].d.ts`
3. add the following to top-level of `tsconfig.json`:
```json
"files": [
  "@types/[nameOfTheThirdPartyModule].d.ts"
],
```
