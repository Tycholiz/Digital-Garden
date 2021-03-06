
### node_modules
- in `node_modules`, we have a folder `.bin/`. This is where the binaries (executables) of our dependencies (listed in `node_modules`) are stored.
	- normally these binaries are symlinks to the binaries that are stored within each package's directory in `node_modules`
	- when are within a project and run a command that is not globally installed, it looks in the `.bin/` directory for that executable
		- ex. running `jest test` in the project will look for an executable called `jest` within `node_modules/.bin` and execute it.
- the [node module resolution algorithm](https://nodejs.org/api/modules.html#modules_loading_from_node_modules_folders) is recursive, meaning when looking for package A, it looks in local `node_modules/A`, then `../node_modules/A`, then `../../node_modules/A`, and so on.
- unless we use strict versioning (ie. omitting `^` in package.json.dependencies) the version listed in package.json is not the version our package uses. Rather, it defines a range that is allowed to be installed. To see the actual version, check yarn.lock

### Package.json
[Adding comments to package.json (of course, a workaround; not actual comments)](https://stackoverflow.com/questions/14221579/how-do-i-add-comments-to-package-json-for-npm-install)
