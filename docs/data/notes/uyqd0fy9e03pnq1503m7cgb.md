
## Module resolution
Typescript can be configured to resolve modules in 2 ways: classic or node.

### Node resolution
With the node-style of resolution, [[Node's module resolution process|js.node.modules#how-node-resolves-modules,1:#*]] is mimicked. Typescript will overlay the ts source-file extensions (ie. `.ts`, `.tsx`, `.d.ts`) over Node's resolution logic.
- TypeScript will also use a field in `package.json` named types to mirror the purpose of "main" - the compiler will use it to find the “main” definition file to consult.

When we `import myFunction from 'moduleB'`, the `moduleB` module will be searched for in a `node_modules` directory at the current level. It will search:
1. `/root/src/node_modules/moduleB.ts`
2. `/root/src/node_modules/moduleB.tsx`
3. `/root/src/node_modules/moduleB.d.ts`
4. `/root/src/node_modules/moduleB/package.json` (if the package.json specifies a *types* property)
5. `/root/src/node_modules/@types/moduleB.d.ts`
6. `/root/src/node_modules/moduleB/index.ts`
7. `/root/src/node_modules/moduleB/index.tsx`
8. `/root/src/node_modules/moduleB/index.d.ts`

If not found, it will go up one level and repeat the process until the module is found or there are none left to check. 

### Debugging module resolution
We can debug the module resolution process by compiling with the `traceResolution` flag:
- `tsc --traceResolution`
    - tip: pipe the output into grep: `tsc --traceResolution | grep module-that-wont-resolve`

Keep an eye out for:
- Name and location of the import
    - `======== Resolving module ‘typescript' from ‘src/app.ts'. ========`
- The strategy the compiler is following
    - `Module resolution kind is not specified, using ‘NodeJs'.`
- Loading of types from npm packages
    - `'package.json' has 'types' field './lib/typescript.d.ts' that references 'node_modules/typescript/lib/typescript.d.ts'.`
- Final result
    - `======== Module name ‘typescript' was successfully resolved to ‘node_modules/typescript/lib/typescript.d.ts'. ========`

### Module resolution flags (tsconfig settings)
Sometimes the layout of the source files does not match the layout of the output. That is, the directory structure (which includes the names of files and where they are located) may differ before and after we compile the Typescript.
- for this reason, the Typescript compiler has a set of additional flags to inform the compiler of transformations that are expected to happen to the sources to generate the final output. That is, these fields will help guide the process of resolving a module import to its definition file.

#### `baseUrl`
Lets you set a base directory to resolve non-absolute module names. All module imports with non-relative names are assumed to be relative to the baseUrl.
- This negates our need to import with `./` and `../`, and instead we can just start from the baseUrl that we specify

If we set `baseUrl: ./`, then with the following directory structure...
```
baseUrl
├── ex.ts
├── hello
│   └── world.ts
└── tsconfig.json
```

Here, we can just `import { helloWorld } from 'hello/world'`

Naturally, `baseUrl` only applies to non-relative imports.

#### `paths` (path mapping)
When using third party modules, they are not directly located under `baseUrl`.
- using `import $ from 'jquery'` would be translated at runtime to `"node_modules/jquery/dist/jquery.slim.min.js"` 
    - note: this is able to be figured out because Loaders use a mapping configuration to map module names to files at run-time.

The Typescript compiler lets us declare mappings like this with the `paths` property.
- for instance, we can specify the above like this;
```json
"paths": {
    "jquery": ["node_modules/jquery/dist/jquery"] // This mapping is relative to "baseUrl"
}
```

Because the value is an array, we can specify multiple places for the Typescript compiler to look for those modules.
- imagine the following directory structure, where some modules are stored in `generated/`, and the rest in another directory. The build step would put them all in the same place.
```
projectRoot
├── folder1
│   ├── file1.ts (imports 'folder1/file2' and 'folder2/file3')
│   └── file2.ts
├── generated
│   ├── folder1
│   └── folder2
│       └── file3.ts
└── tsconfig.json
```

```json
"paths": {
    "*": ["*", "generated/*"]
}
```

This tells the compiler for any module import that matches the pattern "*" (i.e. all values), to look in two locations:
1. "*": meaning the same name unchanged, so map `<moduleName>` => `<baseUrl>/<moduleName>`
2. "generated/*" meaning the module name with an appended prefix `generated`, so map `<moduleName>` => `<baseUrl>/generated/<moduleName>`

Following this logic, the compiler will attempt to resolve the two imports as such:

`import 'folder1/file2'`:
1. pattern '*' is matched and wildcard captures the whole module name
2. try first substitution in the list: '*' -> folder1/file2
3. result of substitution is non-relative name - combine it with baseUrl -> projectRoot/folder1/file2.ts.
4. File exists. Done.

`import 'folder2/file3'`:
1. pattern '*' is matched and wildcard captures the whole module name
2. try first substitution in the list: '*' -> folder2/file3
3. result of substitution is non-relative name - combine it with baseUrl -> projectRoot/folder2/file3.ts.
4. File does not exist, move to the second substitution
5. second substitution 'generated/*' -> generated/folder2/file3
6. result of substitution is non-relative name - combine it with baseUrl -> projectRoot/generated/folder2/file3.ts.
7. File exists. Done.

* * *

Often, in Typescript files we need to do this:
```ts
- import _ from 'lodash';
+ import * as _ from 'lodash';
```

This is due to the fact that there is no default export present in lodash definitions.

To make imports do this by default and keep `import _ from 'lodash';` syntax in TypeScript, set "allowSyntheticDefaultImports" : true and "esModuleInterop" : true in your tsconfig.json file.