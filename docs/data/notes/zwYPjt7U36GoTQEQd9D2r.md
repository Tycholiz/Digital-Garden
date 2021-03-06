
Workspaces allow us to install dependencies from multiple package.json files at once.
- Yarn can also create symlinks between workspaces that depend on each other, ensuring directories are correct and consistent
- When there are repeated dependencies in the `node_modules` of sub-modules, yarn workspaces will pull up (hoist) the common dependencies to live in the root-level `node_modules`.
	- This is why we don't need to run `lerna bootstrap --hoist` when using yarn + lerna — yarn already hoists.
- if package1 depends on package2, then a symlink to package2 will also be created in the root `node_modules`. This allows package2 to `require` in the package, and take advantage of node's recursive resolver to find the package.
- workspaces uses a single yarn.lock file at the root.
- any module (either your own code or the code of a dependency) that contains native code (ie. Swift, Java etc) must not be hoisted.

## How to implement
1. Add a `workspaces` key to the root `package.json`:
- `@app` holds all the modules
```json
"workspaces": {
	"packages": [
		"@app/*"
	]
}
```
2. Change the name field of each module's `package.json` to follow a pattern so that they can be referenced easily
3. Add module script shortcuts to root `package.json`, using the module names as changed in the previous step:
```json
"scripts": {
	"db": "yarn workspace @tycholiz/db",
}
```
4. Add `private: true` property to root `package.json`.
5. Add script in root `package.json` that allows us to execute multiple commands at once (note: consider using `concurrently` package to run servers in parallel)
```json
"scripts": {
    "dev": "concurrently --kill-others-on-fail \"yarn sanity dev\"  \"yarn web dev\"",
}
```
