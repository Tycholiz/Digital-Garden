
## Initializing project
1. In your new project directory, run `go mod init <URL of remote repo>`
2. Add packages with `go get <package URL>`

* * *

### Module
A module is Go’s new dependency management system.

In a module, you collect one or more related packages for a discrete and useful set of functions. 
- Go code is grouped into packages, and packages are grouped into modules.
- a module has a `go.mod` file at its root
  - this file specifies dependency requirements and the module path.
- ex. you might create a module with packages that have functions for doing financial analysis so that others writing financial applications can use your work.

Your module specifies dependencies needed to run your code, including the Go version and the set of other modules it requires.

#### `GOPATH`
`GOPATH` is the deprecated way of managing Go projects.

Before modules, every Go project had to be created in the `GOPATH`
- run `go env GOPATH` to see

- [More about GOPATH](https://golangr.com/what-is-gopath/)

#### Module Path (Name)
When you create a module, you specify a path from which your module can be downloaded by Go tools.
- most often this is our Gitlab/Github URL to our repo.
- The module path becomes the import path prefix for packages in the module, so be sure to specify a module path that won’t conflict with the module path of other modules.

The module path is typically of the form: `<prefix>/<descriptive-text>`
- `prefix` might be your repo name. You would do this if you intended for others to use this module.
  - if the module isn't going to be used by others, then you can just call it by your project name, as long as you are certain it won't be used by others.

### Packages
A Go project can be broken out into packages that get compiled together.
- Functions, types, variables, and constants defined in one source file are visible to all other source files within the same package.
- spec: packages fall along the same lines drawn by [[DDD|general.principles.DDD]], and each package would correspond to each of our domains
  - ex. when making a blog app, packages might be `users`, `articles`

In the most basic terms, a package is nothing but a directory inside your Go workspace containing one or more Go source files, or other Go packages.
- therefore, every Go source file belongs to a package, and we specify a source file as being part of a package by declaring `package <packagename>` at the top.

Other packages can import and reuse the functions or types that are exported from your package.

Go programs start running in the `main` package.

By convention, Executable programs (the ones with the main package) are called *Commands*. Others are called simply *Packages*.

Go’s convention is that - the package name is the same as the last element of the import path. 
- ex. the name of the package imported as `import math/rand` is `rand`. It is imported with path `math/rand` because it is nested inside the `math` package as a subdirectory.
```
.
├── main.go
├── strings/
│   ├── reverse_name.go
│   └── greeting/
|       └── texts.go (`package greeting`)
```
- above, within `main.go`, we would import `texts.go` with `"github.com/tycholiz/my-project/strings/greeting"`

Anything (variable, type, or function) that starts with a capital letter is exported, and visible outside the package.
- therefore, when you import a package, you can only access its exported names.

- [source](https://www.callicoder.com/golang-packages/)



* * *

### Golang Runtime