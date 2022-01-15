
# package.json
## Scripts
- every script we define has `pre` and `post` version to it
    - ex. if we define a `install` script, then there is a `preinstall` that is implicitly defined

## Dependencies
### Peer Dependencies
- peer dependencies are not automatically installed like dependencies and devDependencies are.
	- Instead, when we list a package as a peerDependency, we expect it to be provided from the host (the host would be our application)
- peer dependencies are most likely to be used only when creating npm packages.
- ex. `react-dom` lists `react` as a peerDependency, since `react-dom` is useless unless we have `react` installed.
	- if `react-dom` had listed `react` as a dependency, then effectively we would have 2 different versions of react in our project.
	- by listing `react` as a peerDependency, `react-dom` is basically saying "hey npm, I don't have react as a dependency, but I do need react to work. If someone tries to install me, but they don't have react installed, spit a warning at them, cause I ain't gonna work properly"

[more info](https://flaviocopes.com/npm-peer-dependencies/)


* * *

The scripts section of package.json has access to the commands provided by node_modules. for instance, we can make a script that calls `jest`. We can't do this in our shell because it doesn't have access to the package by default. we'd have to manually go into node_modules to do that. But the commands are loaded into the shell that is openeed when we run scripts in package.json
