
# Substitution
repeat last substitute
- `:&`
    - `:&&` command repeats the last substitution with the same flags.
        - You can supply the additional range(s) to it (and concatenate as many as you like):

replace multiple search terms at once
- `:%s/Kang\|Kodos/alien/gc`

`:6,10s/<search_string>/<replace_string>/g | 14,18&&`

## within Visually Selected Area
1. visually select the area you want substitutions to take place in, then `<ESC>`
2. `:%s/\%Vfoo/bar/g` to replace all `foo` with `bar`

Alternatively, the vis.vim plugin can be used

### Upper/Lower Case
- `guw`/`gUw` - make word lowercase/uppercase
    - `guu`/`gUU` - make line uppercase/lowercase
- `g~` - swap case

## Flags
### c
y/n/a/q substitute (within single file)
- `:%s/search/replace/gc`

#### Values
`l` - last (substitute this match, then quit)
`a` - all (this, and all remaining)
    - note: `l` and `a` are opposites, since `l` will treat the current match as the final one in our substitution process, whereas `a`
`<C-e>`/`<C-d>` - scroll
