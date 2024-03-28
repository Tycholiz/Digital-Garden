
- a pipe takes stdout from one process and passes it as stdin to another
	- if the first program fails, then a non-zero status code will be returned.
- Each command in a pipeline is executed in its own subshell, which is a separate process
