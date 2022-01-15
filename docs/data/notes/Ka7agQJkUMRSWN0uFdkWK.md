
Node.js is built with a modular-first mentality. However, the web has been slow to adopt this philosophy of having clearly defined and swappable modules. Webpack aims to solve this by allowing us to implement modules in our code
- unlike Node, webpack can also treat css/sass files, and image URLs as modules.

Webpack is eager, in that javascript modules are figured out ahead of time, and are delivered in one big file. This negates the need to make network requests to get more of the javascript. The consequence of this fact is:
- we cannot import dynamically in a data-driven way (since the importing will already have been done by the time the data-driven code is run.)
- we cannot import in a function, since a function occurs after webpack does its job.
- Module objects are frozen. There is no way to hack a new feature into a module object, polyfill style.
- import errors are detected at compile-time
- There is no hook allowing a module to run some code before its dependencies load. This means that modules have no control over how their dependencies are loaded.

# Concepts
- each webpack config has an entrypoint from which the dependency graph starts to be made. Unless specified, this defaults to `./src/index.js`
- each webpack config also has an output, which specifies where the bundle will be emitted. Unless specified, this defaults to `./dist/main.js` for the main file, and the rest of the files also end up in `dist/`

## Loaders
- in Webpack 4 and below, webpack only understands js and json files. For this reason, we use loaders to allow different files to form a part of the dependency graph.
- ex. `{ test: /\.txt$/, use: 'raw-loader' }`
	- basically, loaders say "hey Webpack, when you come across `.txt` files whenever you are trying to import, use the `raw-loader` to transform it before adding it to the bundle.
- loaders are declared in `module.rules`

## Types of Modules
- There are more, but the 2 main types of modules we are concerned with are ECMAScript Modules, and Asset Modules

### Asset Modules
- In webpack 5, Asset Modules allow us to use asset files (ie. fonts, icons), without configuring additional loaders.
	- before Webpack 5, we had to use `raw-loader`, `url-loader`, `file-loader` etc., since out of the box, those versions of webpack only understood js and json files.

### Module Resolution
A resolver is a library which helps in locating a module by its absolute path
- Effectively, webpack can be in charge of locating the files, and creating an alias for that path, so that that alias can be used throughout the codebase, giving us the benefit of only having to change that path once, in the event that it needs to change.

* * *
### Resolve
#### resolve.modules
- in webpack.config, we can tell webpack where it should go to look for certain modules.
	- ex. `node_modules` will be a default location, but we can specify paths that have higher priority so they are checked first before `node_modules`
- if relative paths are used, then webpack will scan for that file recursively, similar to how node resolves `node_modules`

#### resolve.alias
- allows us to import more easily in our project.
- we basically tell webpack, "hey, any time you see me `import _ from 'ui-components`, just look for that module at whatever path I specify."

webpack continuously builds our app as we go.

* * *

### Contexts (require.context)
- allows us to create our own context by passing a pattern that will match a filename.
- One of the main functions of webpack's compiler is to recursively parse all modules to form a dependency graph.
- A context is simply a base directory that resolves paths to modules (ie. it is a directory and its dependencies)
	- ex. in a React app, we can choose to declare the `components/` directory a context. This will allow us to search for files within that directory "tree"
- Realistically, the root directory (current working directory) is a context for webpack to actually resolve the path to `index.js`
- The intention is to tell webpack at compile time to transform that expression into a dynamic list of all the possible matching module requests that it can resolve, in turn adding them as build dependencies and allowing you to require them at runtime.
- In short, you would use require.context in the exact same situation when in Node.js at runtime you would use globs to dynamically build a list of module paths to require. The return value is a callable object that behaves like require, whose keys contain the necessary module request data that can be passed to it as an argument to require the module.

# Alternatives
- Snowpack: very fast
- Rollup
- Parcel: simple config that works out of the box
- Vite: Built by Evan You (of Vue.js). Out of the box with Sveltekit

# UE Resources
[Chunking in Webpack](https://medium.com/hackernoon/the-100-correct-way-to-split-your-chunks-with-webpack-f8a9df5b7758)
