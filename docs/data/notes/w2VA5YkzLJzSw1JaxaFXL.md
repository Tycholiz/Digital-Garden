
While literal characters, [[character classes|regex.character-class]], and the `.` match a single character, anchors don't match any character at all. Instead, they match a position before, after, or between characters.
- Anchors are therefore said to be *zero-length*.
- They are called anchors because they can be used to “anchor” the regex match at a certain position

### `^`
`^` matches the position before the first character in the string.
- Applying `^a` to `abc` matches `a`.
- `^b` does not match `abc` at all

### `$`
`$` matches right after the last character in the string.
- `c$` matches `c` in `abc`
- `a$` does not match at all.

### `\b`
matches at a position that is called a “word boundary”
- `\b` allows you to perform a “whole words only” search using a regular expression in the form of `\bword\b`

`\b` is not technically an anchor, but it is very closely related. If we were to imagine a string of text as the space between 2 walls, then we would consider the anchors `^` and `$` to be either wall (ie. the position before the first character, and after the last character). `\b` differs from this because `\b` marks the position of a word character `\w` on one side, and a non-word character `\W` on the other.

There are three different positions that qualify as word boundaries:
1. Before the first character in the string, (if the first character is a word character).
2. After the last character in the string, (if the last character is a word character).
3. Between two characters in the string, where one is a word character and the other is not a word character.

![](/assets/images/2021-11-28-14-22-35.png)

### Using `^` and `$` as Start of Line and End of Line Anchors
Called “multi-line mode”, and it generally needs to be explicitly enabled.

If you have a string consisting of multiple lines, like `first line\nsecond line`, it often makes sense to think in terms of lines, rather than the entire string
- ex. `^` can match at the start of the string (ie. before the letter `f`), as well as after each line break (between `\n` and the letter `s`)
- ex. Likewise, `$` still matches at the end of the string (after the last `e`), and also before every line break (between `e` and `\n`)

* * *

Anchors are very important when using regex in a programming language to validate user input
