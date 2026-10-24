
- Single quotes `''` - preserves the literal value of each character within the quotes (don't allow anything to be interpolated inside). Take it exactly as it appears.
	- ex. if `$MYVARIABLE` is within the `''`, the string `$MYVARIABLE` will be interpreted.
- Double quotes `""` - evalutate the functions within, interpolating into the string
	- ex. if `$MYVARIABLE` is within the `""`, the value that `$MYVARIABLE` stands for will be interpreted.

nm: double quotes are bigger, so they have more capabilities (the ability to interpret and expand variables)

```sh
# NO STRING INTERPOLATION
$ echo '$(echo "upg")'
# $(echo "upg")

# js equivalent:
# console.log("$(echo "upg")")

# STRING INTERPOLATION
$ echo "$(echo "upg")"
# upg

# js equivalent
# `${}`
```
- The implication of this is that when we use double quotes, since the inerpolated functions are called when defined, the variable that gets set to the string will only be executed once.
- ex:
```sh
PROMPT="$(git_status) $"
# defining this variable will cause git_status to run, interpolating the return
# value of that function. The only way to update this variable, is by sourcing .zshrc

PROMPT='$(git_status) $'
# here, we literally pass the string '$(git status)' to the shell (?),
# and let it interpolate (?) on its own, each time it is displayed in the terminal
```

## Globbing
- quotes used in a globbing context do not behave as described above. Globs are not expanded when in either single or double quotes
