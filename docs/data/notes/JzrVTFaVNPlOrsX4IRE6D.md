
in `node_modules`, we have a folder `.bin/`. This is whe
- normally these binaries are symlinks to the binaries that are stored within each package's directory in `node_modules`
- when are within a project and run a command that is not globally installed, it looks in the `.bin/` directory for that executable
	- ex. running `jest test` in the project will look for an executable called `jest` within `node_modules/.bin` and execute it.

### How Node resolves modules
When we `import myFunction from 'moduleB'`, the `moduleB` module will be searched for in a `node_modules` directory at the current level. It will search:
1. `./node_modules/moduleB.js`
2. `./node_modules/moduleB/package.json` (if it specifies a "main" property)
3. `./node_modules/moduleB/index.js`

if it doesn't find the module here, it will go up one level and continue the search in the same pattern
1. `../node_modules/moduleB.js`
2. `../node_modules/moduleB/package.json` (if it specifies a "main" property)
3. `../node_modules/moduleB/index.js`

And so on, until the module is either found or there are no more levels to search.

the [node module resolution algorithm](https://nodejs.org/api/modules.html#modules_loading_from_node_modules_folders) is recursive, meaning when looking for package A, it looks in local `node_modules/A`, then `../node_modules/A`, then `../../node_modules/A`, and so on.
- unless we use strict versioning (ie. omitting `^` in package.json.dependencies) the version listed in package.json is not the version our package uses. Rather, it defines a range that is allowed to be installed. To see the actual version, check yarn.lock



### Package.json
- [Adding comments to package.json (of course, a workaround; not actual comments)](https://stackoverflow.com/questions/14221579/how-do-i-add-comments-to-package-json-for-npm-install)

## UE Resources
- [Understanding circular dependency issues](https://medium.com/visual-development/how-to-fix-nasty-circular-dependency-issues-once-and-for-all-in-javascript-typescript-a04c987cf0de)