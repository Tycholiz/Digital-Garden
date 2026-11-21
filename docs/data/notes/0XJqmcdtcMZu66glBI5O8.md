
Lerna allows us to have multiple packages within our project. Complete with each package will be a `package.json` and a corresponding `node_modules` directory

We can list one of our packages as a dependency of another one by including the package in the `dependencies` section of the dependent's `package.json` 

The root of a project with Lerna will have a `lerna.json`, as well as a `packages/` directory
- Each sub-package of our project must be within the `packages/` directory 

- To run `lerna bootstrap` is to install all dependencies in sub-modules
- When we run `bootstrap`, lerna will call `yarn` in each module, then create symlinks between the packages that refer to each other in the dependent's `node_modules`
	- ex. If we have 3 modules: Addition, Subtraction, and Calc (which performs the add/subtract operations), then Calc's `node_modules` will contain symlinks to the Addition and Subtraction modules. 
- if we use hoisting, then symlinks don't play a part. Instead, if we have `react` in 2 different submodules, then hoisting will allow us to remove `react` from those submodules, install it at the root level, then allow the node recursive resolver to handle resolution for us. In other words, from within the submodule, when we require `react`, it will look for the package in its nearest `node_modules`, not find it, then continue upwards until it does, which will be at the root.
	- The reason why hoisting works is due to the resolve algorithm of node require

### Duplication
- naturally, having multiple sub-packages in a project will result in duplicate package listings. Lerna offers hoisting, which allows us to effectively list the same package in multiple places, but have the packages install at the root level
	- therefore, the duplicated package (ex. React) will be in the root directory's `node_modules`, even though it is listed in the sub-package's `package.json` 
- If we have a project with 2 sub-modules: A and B, and both have React as a dependency, we can run `lerna bootstrap --hoist` at the root, which will remove React (and all of its dependencies) from `A/node_modules` and `B/node_modules`, and move them to the root level `node_modules`. Because of the recursive nature of how `node_modules` are resolved, this will cause the sub-module's package.json to look upwards in the directory hierarchy until the react binary is found. 
	- Lerna will warn us when running this command if we have version mismatches

### Commands
- `bootstrap` - generate `node_modules` for packages.
- `clean` - remove all `node_modules` that are not in root directory
- `create` - add a new sub-package to your project
- `run` - run the script that is listed in each sub-`package.json`
	- Therefore, it will run only npm or yarn commands (with the script that is listed in package.json)
- `exec` - run a command inside each package
	- similar to `run`, but is not restricted to running scripts in `package.json` 

### Misc
You don't actually need to run lerna bootstrap if you're using yarn workspaces.
