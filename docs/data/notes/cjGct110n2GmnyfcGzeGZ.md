
# Patterns
- `ack <search-string> ../src`

# Flags
- `-i` - ignore case
- `-w` - only match whole words
- `-1` - stop searching after 1 match found
- `-Q` - treat all characters as literal
    - so if we use `/w` in the pattern, it is taken literally and will not mean "match word only"

# Types
- `ack --react <PATTERN>` - search all files with type react (js, jsx)
- `--help-types` - see all types defined
- `ack 'my pattern' ./src` - search for *my pattern* in *src* directory

# Regex (Ag)
- Ag is magic by default, meaning if we want to match literal characters, we need to escape them. all letters will be literal, and will need to be escaped to get their special meaning (ex. `\d`, `/s`)
    - ex. ag '.*\ddog' - will match `sdfhj5dog`
        - anything any number of time, followed by a number, followed by 'dog'
- see `man pcre2pattern` for regex flavors

All devDependencies should be at the root
everything we need to build the project is at the root
