
# Macros
- allow us to record and number of keystrokes into a register
- ideal for repeating changes over a set of similar lines, paragraphs and files.
- macros allow us to easily append commands to the end of an existing one
    - for complex ones, we can paste the macro into a document, edit it, then yank it back into the register
- golden rule - ensure every command in a macro is repeatable
- before starting to record a macro, ask: where am I, where did I come from, where am I going?
    -  before doing anything make sure the cursor is positioned to the next command does as we'd expect, and where we'd expect
- any motions that fail cause the macro to fail.
    - ex. - pressing `k` on line 1 does nothing, so it would fail if we tried.
- `@@` - repeat last macro

When you are making a macro, imagine you are looping a piece of paper back onto itself. The edge you start at must touch the edge you finish at. This is the same concept as making macros: After identifying a repetitive action, define the start point of a single action, start recording and do the stuff, then finish the macro when you are at the start of the next action.

## Tips

When using macros to perform a repetitive action on multiple lines, it's a good idea to make the first action to move our cursor to the starting position (ie. the edge in the paper loop analogy)
- ex. we can start with `0` to go to the start of the line
