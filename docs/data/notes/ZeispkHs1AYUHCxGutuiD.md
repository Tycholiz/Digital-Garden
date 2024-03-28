
`m[a-zA-Z]` marks the current cursor location with the designated letter
- lowercase marks are local to the buffer, while uppercase are globally accessible
    - therefore, if we have 5 buffers open, each buffer can have mark "a", but only one can have mark "A"

- **`a** will jump to the exact spot of *mark "a"*
    - *mn.* - more precise, just like js template literals (which use back ticks)
- **'a** will jump to the **line** of *mark "a"*
    - more useful in the context of an *ex command*
- **`** and **'** will both jump to marks. **'** will take you to the line, and **`** will shoot you to the extact spot.

## Automatic marks
- vim automatically sets up some marks for us:

| Keystroke | Buffer Contents                               |
| --------- | --------------------------------------------- |
| "         | Position before last jump within current file |
| 0         | Position of cursor when the file was last closed|
| [         | Start of last change/yank                     |
| ]         | End of last change/yank                       |
| <         | Start of last visual selection                |
| >         | End of last visual selection                  |
| 1, 2..    | latest position of cursor in last file opened |
