
### yarn.lock
- The purpose of a lock file is to lock down the versions of the dependencies specified in a package.json file
	- This means that in a `yarn.lock` file, there is an identifier (ie. exact version specified) for every dependency and sub-dependency that is used for a project
		- sub-dependencies are the dependencies of a dependency
- The equivalent of `yarn.lock` for npm is `package-lock.json`. If using both npm and yarn, we need both of them, and they need to remain in sync (use yarn's import directive to accomplish this)
- if we didn't have a `yarn.lock`, then if a co-worker cloned our repo and ran `yarn install`, they may get different versions of a dependency, since `package.json` can specify version ranges. 
	- Instead, since `yarn.lock` is checked into version control, when the co-worker clones the repo and runs `yarn install`, `yarn.lock` will be checked and the version specified will be installed.
- critical to have if working on a team or if working alone with a CI server.
- `yarn.lock` gets updated any time a dependency is added, removed or modified
	- If we want to ensure `yarn.lock` is not updated, use `--frozen-lockfile`
		- The difference between `--frozen-lockfile` and `--pure-lockfile` is that the former will fail if an update is needed 
- In a perfect world, yarn.lock is unnecessary, because the point of semver is that unless the major version changes, the upgraded package will still work. In other words, if the version in package.json is listed as ^16.0.1, then `yarn install` is free to go to the latest minor version, which doesn't matter since semver defines that as fully backwards compatible.
	- however, in the real world not everyone follows semver best practices, and sometimes it is just mistakes which ruin backward compatibility 

#### Upgrading packages
- if we have a dependency version in `package.json` specified at `^3.9.1`, this means that any version between 3.9.1 and 4.0.0 will be acceptable. Of course, since we have a lockfile, upgrades will not automatically happen.
- `yarn upgrade` allows us to upgrade all dependencies in `package.json`. If we use the `^` specifier, then the latest version within the range will be added. This will be reflected in `yarn.lock`
- we can ignore the version range by passing the `--latest` flag.
	- This modifies both `yarn.lock` and `package.json`
- We can see all packages that can be upgraded with `yarn upgrade-interactive --latest`

### Link
- `yarn link` allows us to create symlinks to local projects, from within the project (with package.json) we are currently in.
- ex. if we have a `rn-client` project and a `components` project, and we want to use `components` within `rn-client`, we can do the following:
	1. go to `components` project and run `yarn link`
	2. go to `rn-client` project and run `yarn link components` (name field of package.json)
		- this creates a symlink at `rn-client/node_modules/components`
	4. from `rn-client` project, `import components from 'components'` 
- It is meant for development-only purposes
- spec: think of `yarn link` as exporting the package, and `yarn link <package>` as importing it.
