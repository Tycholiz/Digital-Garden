
## Scripts
Every script we define has `pre` and `post` version to it
- ex. if we define a `install` script, then there is a `preinstall` that is implicitly defined

## Dependencies
- `^2.2.3` means `>=2.2.3` AND `<3.X`

### Peer Dependencies
peer dependencies are not automatically installed like dependencies and devDependencies are.
- Instead, when we list a package as a peerDependency, we expect it to be provided from the host (the host would be our application)

peer dependencies are most likely to be used only when creating npm packages.
- ex. `react-dom` lists `react` as a peerDependency, since `react-dom` is useless unless we have `react` installed.
	- if `react-dom` had listed `react` as a dependency, then effectively we would have 2 different versions of react in our project.
	- by listing `react` as a peerDependency, `react-dom` is basically saying "hey npm, I don't have react as a dependency, but I do need react to work. If someone tries to install me, but they don't have react installed, spit a warning at them, cause I ain't gonna work properly"

[more info](https://flaviocopes.com/npm-peer-dependencies/)

### Alias package names
We can alias our package names however we want to control how we import them in our code:
```json
"pouchdb-core-react-native": "npm:@craftzdog/pouchdb-core-react-native@7.2.2",
```

Here, we are using the npm protocol to specify which package we'd like to install, and we set it to `pouchdb-core-react-native`.
- this can be helpful if we are trying to get DefinitelyTyped [[declaration files|ts.declaration-file]] to work with a package where the name differs from the DefinitelyTyped version (which would occur, for example, if we were using a fork of the library, which we are in the above example)

## Resolutions
`resolutions` is simply a map of package names and the exact versions of those packages that should be kept in the dependency tree

- ex. this configuration will remove all versions of webpack that are not 5.6.0. As long you install webpack@5.6.0 version as a dependency of the project you are working with, this will guarantee that all packages load the same version of webpack.
```json
{
	"resolutions": {
		"webpack": "5.6.0"
	}
}
```

* * *

The scripts section of package.json has access to the commands provided by node_modules. for instance, we can make a script that calls `jest`. We can't do this in our shell because it doesn't have access to the package by default. we'd have to manually go into node_modules to do that. But the commands are loaded into the shell that is openeed when we run scripts in package.json
