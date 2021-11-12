
#### See the command attached to a bash alias
run `type <command>`

#### Run multiple commands in sequence
```sh
$ command1; command2
```

This is distinct from `&&`, since commands will be run regardless of exit status

#### Run previous command with `sudo` in front
```sh
╰─➤ $ rm -rf /var/www
rm: /var/www: Permission denied
╰─➤ $ sudo !!
╰─➤ $ sudo rm -rf /var/www
```
