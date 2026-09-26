
An environment is created every time a shell is initialized
an environment is just a map of key-value pairs
- Each command is executed in its own environment, which includes (but not limited to):
	1. files that have been sourced
	2. current working directory
	3. functions defined during execution, or inherited from shell's parent in the environment
- When a non-builtin command is executed, it is invoked in a separate execution environment.

## Environment Variables
Since every instance of a shell is a separate process, we have a different set of environment variables in each shell
- they can be seen by running `env`
	- That isn’t all the variables that are set in your shell, though. It’s just the environment variables that are exported to processes that you start in the shell.
- `compgen -v` allows us to see *all* variables available in shell
- `export` allows us to add parameters and functions to the environment
- When a non-builtin command is executed, it is invoked in a separate execution environment.

### Export
- Exported variables get passed on to child processes, not-exported variables do not.
	- When we use `export` in bash, we are adding the variable onto the shell's list of all env variables. This list is exclusive to the shell. When this shell creates a child process, all of these env variables are made available to it.
	- This means that if we only need the variable in the current environment, then we don't need to use `export`

the environment variables that an application can see are based on how the application was launched (from the dock, from the commandline, etc)
