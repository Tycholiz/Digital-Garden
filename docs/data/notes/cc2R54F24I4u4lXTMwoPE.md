
## Modifications
- `!` before the flag to check opposite 
    - ex.  `if [ ! -f "$FILE" ]` checks that file does NOT exist

* * *

### Check if a CLI package is installed
```sh
if ! aws_bin="$(which aws)" 2>/dev/null; then
    echo "aws cli is missing; you can get it from https://aws.amazon.com/cli/"
    exit 1
fi
```

#### Variation: install package if it's not found
```sh
[ $(which ts-node) ] || echo "ts-node not found. Installing..." && npm i -g ts-node
```

### Check if environment variable set
```sh
if [ -z $APP_NAME ]; then echo "Error:'APP_NAME' is not set, it is required for serverless" && exit 1; fi
```


### Check if X exists
- file - `-f`
- directory - `-d`

```
if [ -d "$FILE" ]; then
    echo "$FILE is a directory."
fi
```

One liner:
`[ -d /etc/docker ] && echo "$FILE is a directory."`