
Open3 grants you access to stdin, stdout, stderr and a thread to wait for the child process when running another program.
- You can specify various attributes, redirections, current directory, etc., of the program in the same way as for Process.spawn.

#### `capture2`
captures the standard output of a command.
- `2` refers to stdout
```rb
stdout_str, status = Open3.capture2([env,] cmd... [, opts])
```
