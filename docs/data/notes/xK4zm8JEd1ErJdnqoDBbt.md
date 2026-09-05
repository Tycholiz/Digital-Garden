
### See the command attached to a bash alias
run `type <command>`

### check to see if alias exists in shell
run `which g`

### Run multiple commands in sequence
```sh
$ command1; command2
```

This is distinct from `&&`, since commands will be run regardless of exit status

### Run previous command with `sudo` in front
```sh
╰─➤ $ rm -rf /var/www
rm: /var/www: Permission denied
╰─➤ $ sudo !!
╰─➤ $ sudo rm -rf /var/www
```

### Set a timer to report how long an operation took
```sh
start=$SECONDS
# do some operations
duration=$(( SECONDS - start ))
echo "build and bundle completed in ${duration}s"
```
