
# Visual Mode
reselect last v-mode selection
- `gv`

go to the *other* end of the v-mode selection
- `o`

when using *dot command* to repeat a v-mode command, it acts on the same amount of text as was marked by the recent visual selection

## Tips
- use visual block mode to create multiple cursors to edit multiple places at once
    - ***ex.*** - to append `;` to end of three lines (hence the `jj`), `<C-v>jj$A;`
- when visually selecting by motion (ex. `vi(`), you can increase the level of enclosing parens that you navigate
    - for example, if we have nested parens, we can start off with the above command, then say `i(` to go an additional layer
- When using visual block to edit multiple places, you must use a command that puts you in i-mode(eg. `c`, `I`, `A`)
    - note the capitalization of `I` and `A`. This shows that in visual block mode, the commands to "insert at start" and "insert at end" consider the demarcations to be what is within the visual block (instead of the start/end of the entire line, as is normally the case)
