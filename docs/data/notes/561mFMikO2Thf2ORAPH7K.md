
### Purpose
- to help decide which parts of a program need to be recompiled.
- Makefiles allow us to give a series of instructions to run depending on what files have changed.
- similar to the sript section of the `package.json`

### Structure
- a Makefile consists of a set of rules, which take the form:
```
targets: prerequisites
   command
   command
   command
```
- `targets` are filenames.
- `commands` are a series of steps, normally used to make the target(s)
- `prerequisites` are dependencies that are needed before the commands can be run.

# ## Running "make" command
- when we run `make` without arguments, the first target that doesn't begin with `.` is processed. This is known as the *default goal*.
	- To do this, it may have to process other targets, specifically ones that the first target depends on.
- Often the *default goal* is called `all`, though this is just a convention.

#### Building from source with `make`
1. run `make`
2. run `sudo make install`

### Syntax
- `PG_CONFIG ?= pg_config` - set PG_CONFIG variable only if it isn't already set

## UE Resources
- [Makefile Tutorial](https://makefiletutorial.com/)
