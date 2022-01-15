
# Operators and Motions
- Operator --> Number --> Motion

## operator (Verb)
- what we are doing
- ***ex.*** - `d`, `c`, `y`, `f`, `t`
    - `f#`/`F#` - (*mn.* find)
        - inclusively go to first occurrence of `#` in the line
        - `;` to cycle through, `,` to go backwards
    - `t#`/`T#` - (*mn.* til)
        - exclusively go to first occurrence of `#` in the line

## Modifier
- more specific information about the modifier
- ***ex.*** - `i` (inside), `a` (around)

Combine with numbers to further specify text objects
- ***ex.*** - `d2f:` - delete until the second `:` character
- ***ex.*** - `d2/:<cr>` - delete until the second `:` character

## motion (noun) - what we are operating on
- ***ex.*** - `$`, `0`, `w`, `e`, `b`

* * *

# strat submodules
Structure the vim module from the perspective of a user who is trying to accomplish a certain task in vim.
- Are they trying to substitute all occurrences in a file?
- are they trying to modify text in some way (surround, uppercase, )

- nav (moving the cursor around)
    - line
        - search
        - find
    - buffer
        - search
    - windows
    - tree
        - netrw
        - MRU
        - <C-6>
- modify
    - surround
    - substitute
        - greplace
    - uppercase
- feat
    - marks
    - macro
    - shell (execute shell commands)
- plug
    - fzf
    - fugitive
    - guter (git-gutter)
- repeating
