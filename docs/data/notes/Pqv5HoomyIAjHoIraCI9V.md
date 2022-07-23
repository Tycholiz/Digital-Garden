
`xargs` reads items from stdin, and runs a command (default `echo`) on each one
- each item may either be delimited by:
    1. blanks (which can be protected with double or single quotes or a backslash))
    2. newlines

#### Handling blanks and newlines
Because Unix filenames can contain blanks and newlines, this default behaviour is often problematic; filenames containing blanks and/or newlines are incorrectly processed by `xargs`.
- in these situations pass `-0` option, which would prevent these problems, though we need to ensure that the program which produces the input for `xargs` also uses a null character as a separator
