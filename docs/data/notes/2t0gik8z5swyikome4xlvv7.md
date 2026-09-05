
### pre-commit hook to check for existence of `TODO` in code
```sh
#!/bin/sh

. git-sh-setup  # for die 
if git-diff-index -p -M --cached HEAD -- \
| grep '^+' \
| grep TODO; then
    die Blocking commit because string @todo detected in patch
fi
```