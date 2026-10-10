
### Passing variables to a command
To pass a variable to a shell command, just set it before the command:
- ex. `MY_VAR=123 ./my-script.sh`
- note: the variables only become available to the command that follows. For instance, the variables will not be available in the `my-second-script` file in this case:
```sh
MY_VAR=123 ./my-script.sh && ./my-second-script.sh
```

### Conventions
variables should be **snake_case**
- kebab-case does not work

