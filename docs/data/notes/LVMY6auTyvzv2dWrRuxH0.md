
find empty lines (only whitespace)
- `^\s.*$`

find lowercase/non-lowercase
- `\l`/`\L`

find uppercase/non-uppercase
- `\l`/`\L`

find word boundary
- `\<term\>`

find both "dog" and "Dog"
- `/[Dd]og`

### Greedy vs Non-greedy Quantifiers
Vim allows you to specify how many of something you wish to match by adding quantifiers to the term.
- "greedy quantifiers" are those that match as many of the specified characters as possible.
    - `*` matches 0 or more
        - ex. `/abc*` will match abc, abby, and absolutely
    - `\+` matches 1 or more
        - ex. `/abc\+` will match abc, but not abby or absolutely
    - `\{5,}` matches 5 or more characters
- "non-greedy quantifiers" are those that match only the specified number, or as few as possible.
    - `\=` matches 0 or 1
        - ex. `/abc\=` will match abc, abby, and absolutely.
    - `\{0,10}` matches between 0 and 10 of a character
    - `\{5}` matches 5 characters
    - `\{,5}` matches 5 characters or less
    - `\{-}` matches as little as possible
