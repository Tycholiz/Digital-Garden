
a.k.a metacharacters

There are 12 characters with special meanings:
- `\`
- `^`
- `$`
- `.`
- `|`
- `?`
- `*`
- `+`
- `(`
- `)`
- `[`
	- this usually denotes the start of a custom [[character class|regex.character-class]].
	- in Javascript regex, if we want to match a literal `]` inside a character class, we need to escape it.
- `{`
	- to type a literal `{`, most flavors of regex don't require us to escape with `\`, unless it is part of a repetition operator (e.g. `{1, 3}`)

Inside character classes, the only special characters are `-`, `^`, `\`, `]`
- `^` is only special if it appears at the start of the character class, ie. `[a-z^]` will match a-z and a literal `^`.
- `[- /.]` matches `-`, ` `, `/` or `.`

### Dot `.`
Matches any character but newline

If we wanted to match any character including newlines, we could do `[\s\S]`

A negated character class is often more appropriate than the dot.

ex. consider the text: "Type 1" errors and "Type 2" errors
- If we want to match inside quotes, we can negate `"` as if to say "continue matching characters as long as it's not a `"` character
- `"[^"]*"`
