
# Man Pages
- anything in `[]` brackets indicates optional
- `--` - signify end of command options. Only positional params are accepted after this point.
    - ex. say we want to grep for the string `-v`. If we just executed `grep -v file.txt`, the `-v` would be interpreted as an argument on grep. If we execute `grep -- -v file.txt`, `--` tells us "ok, that's it. No more arguments accepted". Since the section after the args section is the pattern section, `-v` gets interpreted as a pattern.

### Valid command form
- angle brackets for required parameters: ping <hostname>
- square brackets for optional parameters: mkdir [-p] <dirname>
- ellipses for repeated items: cp <source1> [source2…] <dest>
- vertical bars for choice of items: netstat {-t|-u}
