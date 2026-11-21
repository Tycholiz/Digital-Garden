
## High-level
- A shell program is typically an executable binary that takes commands that you type and (once you hit return), translates those commands into (ultimately) system calls to the Operating System API.
	- a shell language is a language designed to ‘glue’ together other programs.
- a shell is simply a macro processor that executes commands.
	- The term macro processor means functionality where text and symbols are expanded to create larger expressions.

Since other shells are also programs, they can be run from within one another

*"Although most users think of the shell as an interactive command interpreter, it is really a programming language in which each statement runs a command. Because it must satisfy both the interactive and programming aspects of command execution, it is a strange language, shaped as much by history as by design."*
- GNU specifies the shell as both a programming language AND a command interpreter. The command interpreter part provides the user interface to the GNU utilities, while the programming language part allows these utilities to be combined.

### Piping
Shell languages are exceptional at one thing: piping
- The `|`, `&` and `;` operators, plus `()` and ``` form a tidy little language for describing pipelines.
	- `a & b` is concurrent
	- `a ; b` is sequential
	- `a | b` is a pipeline where a feeds b

- Think of `( a & b & c ) | tee capture | analysis` as the kind of thing that's hard to express in Ruby (or Python).

- environment variables exist in the same environment as the code that is executed

### Precedence of Synonyms
1. Alias
2. Keyword (ie. syntax of the shell)
3. Function
4. Builtin
	- commands that are built into the shell. These are executed directly, as opposed to the shell having to load and execute the executable
		- ex. pwd, cd

* * *

*interactive* means that input is accepted from the command line, while *non-interactive* means that input is accepted from a file.

if the first word of a given command does not correspond to a built-in shell command, then the shell assumed that it is the name of an executable file.

when we run `bash -c`, a script, or other executable in the shell, it becomes a child of the current environment.
- ex. `bash -c 'echo "child [$var]";` will only have access to `$var` if it was exported beforehand from the shell that the command is run in.

In bash utilities, the `--` signifies the end of command options, after which only positional parameters are accepted.
- often, `--` can signify the separation from the command options, and the following Regex

### Status code 127
Think of it as "Error: the program you tried to use was not found"
- it's returned by your shell `/bin/bash` when any given command within your bash script or on bash command line is not found in any of the paths defined by PATH system environment variable.
- to fix, make sure the command we are using is discoverable through `$PATH`

### Subshell: `$()`
when we execute commands within `$(...)`, they are executed in a subshell

## UE Resources
[GNU High-quality documentation](https://www.gnu.org/software/bash/manual/bash.html)
