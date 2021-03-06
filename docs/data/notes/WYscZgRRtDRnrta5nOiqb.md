
Sed is a steam editor
- here a steam can be thought of as a body of text
- sed works by reading text line-by-line into a [[buffer|memory.buffer]]. For each line, it performs pre-defined instructions, if applicable.
- Sed helps us automate the same sort of tasks that we'd accomplish manually by opening a textfile and making manual, predictable edits.
- like Vim, if we substitute without the `g` flag, then only the first occurrence on the line will substituted.
- though `/` is the most common delimiter, we can use (almost) anything, like `|` or `:`. This is helpful if the `/` is part of the pattern we want to substitute.

Word-Boundary Expression
- use `\b` to disallow partial-word matches
	- ex. we want to replace `foo` with `kyle`, but want to leave `foobar` alone: `sed -i '' 's|\bfoo\b|kyle|g file.txt'`

![[dendron://code/unix.cli.sed.formulas]]

### UE Resources
https://www.brianstorti.com/enough-sed-to-be-useful/
https://linuxize.com/post/how-to-use-sed-to-find-and-replace-string-in-files/
