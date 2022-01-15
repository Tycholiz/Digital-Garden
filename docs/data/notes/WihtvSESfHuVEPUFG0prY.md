
Vim remembers the locations where changes occurred. 
- Each position (column number, line number) is recorded in a change list, and each buffer has a separate change list that records the last 100 positions where an undo-able change occurred.
- if you make a change at a certain position, then make another change nearby, only the location of the later change is recorded 
    - "nearby" means on the same line, within a certain number of bytes.

## Usage
- Type g; to jump back to the position of the previous (older) change.
- Type g, to jump to the position of the next (newer) change.

Type `:changes` to view the list
