
# Pipeline
- a pipe takes stdout from one process and passes it as stdin to another
	- spec: if the first program fails, then a `0` will be passed as stdout. (maybe not true)
- Each command in a pipeline is executed in its own subshell, which is a separate process
