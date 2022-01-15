
"Test" is a conditional check in Bash

`[ -e FILE ]` - True if FILE exists
`[ -d FILE ]` - True if FILE exists and is a directory.
`[ -f FILE ]` - True if FILE exists and is a regular file (ie. not a directory).
`[ -z STRING ]` - True if the length if "STRING" is zero.
`[ -n STRING ]` or `[ STRING ]` - True if the length of string > 0
    - use to check for existence of variable

```sh
http_status=$(curl ___)
if [ $http_status != "200" ]
    then
        echo "command failed"
fi
```

### Test based on exit status of previously executed command
- The `?` variable holds the exit status of the previously executed command (the most recently completed foreground process).
```sh
if [ $? -eq 0 ]
then echo 'That was a good job!'
fi
```
