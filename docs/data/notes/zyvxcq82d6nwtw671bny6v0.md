
### Run the Go program
`go run main.go`

### Initialize a Go module
`go mod init <URL_of_repo>`
- URL of repo is the convention, but not a requirement as long as it's unique

### Compile the code into an executable
run `go build` in the project's directory

### Compile code into an executable and put into `bin/` directory
`go install`

- note: if our project uses 3rd party dependencies (ie. not from standard lib), then it would compile and cache those dependencies into a package directory.
- note: the `bin/` directory here is a sibling directory to the project's `src/` directory