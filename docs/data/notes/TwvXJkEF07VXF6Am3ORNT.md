
## Formatting a command
- anything in `[]` brackets indicates optional
- `--` - signify end of command options. Only positional params are accepted after this point, such as which file will be targeted for the operation.
    - ex. say we want to grep for the string `-v`. If we just executed `grep -v file.txt`, the `-v` would be interpreted as an argument on grep. If we execute `grep -- -v file.txt`, `--` tells us "ok, that's it. No more arguments accepted". Since the section after the args section is the pattern section, `-v` gets interpreted as a pattern.

### Valid command form
- angle brackets for required parameters: `ping <hostname>`
- square brackets for optional parameters: `mkdir [-p] <dirname>`
- ellipses for repeated items: `cp <source1> [source2…] <dest>`
- vertical bars for choice of items: `netstat {-t|-u}`

### Unix built-ins 
use the `manbash` command to see man pages for built-ins (e.g. `cd`, `eval`)
- [source](https://unix.stackexchange.com/questions/18087/can-i-get-individual-man-pages-for-the-bash-builtin-commands)