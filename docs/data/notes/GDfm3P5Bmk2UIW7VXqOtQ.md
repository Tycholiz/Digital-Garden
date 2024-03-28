
Backreferences match the same text as previously matched by a capturing group (with are made with parentheses `()`).
- `\0` references the whole search, `\1` references the first capture group, `\2` the second group, and so on.

use `%` to not capture the following parentheses
- imagine we want to find and replace all occurrences of a first and last name, then replace it by the format `LAST, FIRST`. Notice the where we use and omit the `%` in order to control which matches are going to the `\1` and `\2` registers. Here, we don't care whether "Drew" or "Andrew" was matched, so we don't bother registering it.
	- `/\v(%(And|D)rew) (Neil)`
	- `:%s//\2, \1/g`
- Another resource says that the way to negate is with `?:` immediately after the opening `(`.
	- ex. `color=(?:red|green|blue)`

Surround all instances of a given word with `""` (using regex w/ vim replace)
- `:s/(dog)/"\1"`

- ex. Suppose you want to match a pair of opening and closing HTML tags, and the text in between. By putting the opening tag into a backreference, we can reuse the name of the tag to let us match the closing tag: `<([A-Z][A-Z0-9]*)\b[^>]*>.*?</\1>`
	- note: this doesn't seem to work

To figure out the number of a particular backreference, scan the regular expression from left to right. Count the opening parentheses of all the numbered capturing groups.
