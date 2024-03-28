
`eval` will run the arguments as a command in the current shell
- variables will be expanded before executing, so we can execute commands saved in shell variables

Sometimes we have bash commands that output shell commands (ex. ssh-eval)
We could run these outputted shell commands in 2 steps:
1. run the command
2. take the output of that command, and run it in the shell

Alternatively, we can just wrap the command in `eval`
- ex. `eval $(ssh-agent)`

### Say Hello example
say-hello.sh
```sh
echo "echo hello"
```

```sh
$ ./say-hello.sh
> echo hello

$ eval $(./say-hello.sh)
> hello
```
