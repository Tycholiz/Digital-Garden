
#### Partial Word Search
- When you're typing and you enter a partial word, you can cause Vim to search for a completion by using the `<C-p>` (search for previous marching word) and `<C-n>` (search for next match).

### Where you land
Searches in Vim put the cursor on the first character of the matched string by default
- if you search for Debian, it would put the cursor on the D.

We can offset the cursor placement in a few ways:
- To land on the last character in the matched string, rather than the beginning, add an /e to your search:
    - ex. `/Debian/e`
- To land 2 lines above/below, add `-2`/`+2` to the end
    - ex. `/Debian/-2`
- To offset 2 chars from the beginning of the string, add `b+3`
- To offset 2 chars from the end of the string, add `e-3`


## Tips
- use search with an operator to find the location easier
    - ex. Yank til `})` 
        - `y/})<Enter>`
