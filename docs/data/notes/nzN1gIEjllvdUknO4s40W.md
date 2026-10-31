
A Character Class (CC) is how we tell the regex engine that we want to accept only a subset of all characters
- ex. I want all letters from a-d, and f-z
- ex. I want all alphanumeric characters

A CC stands in for a single character. We can use [[quantifiers|regex.quantifiers]] to match the CC multiple times.

### Built-in Character Classes
whitespace char: `\s`
- identical to `[ \t\r\n\f]`
- e.g. tab, space, newline, carriage return, form feed
digit: `\d`
- identical to `[0-9]`
hex-digit: `\x`
word character (alphanumeric + underscore): `\w`
- identical to `[A-Za-z0-9_]`
alphabetic char: `\a`
lowercase char: `\l`
uppercase char: `\u`
tab char: `\t`
carriage return: `\r`
new line: `\n`
vertical/horizontal whitespace: `\v`/`\h`
- only available in Perl Regex; if available, these are preferable to `\s`

#### Inverse
To [[inverse|dendron://thoughts-on/math.function.inverse]] the match, we just need to uppercase the character
- ex. `\s` matches a whitespace character, `\S` matches a non-whitespace char.

### Custom Character Classes
Letter from a-d, and f-z
- `[a-df-z]`

#### Inverse
To [[inverse|dendron://thoughts-on/math.function.inverse]] the whole character class, we just need to put `^` in front
- ex. `[^0-9a-z]` matches any character that isn't numeric or lowercase

note: `[\D\S]` is not the same as `[^\d\s]`
- the first matches any character that is either not a digit, or is not whitespace. Therefore, matches any character; since all digits are non-whitespace, and all whitespace characters are not digits
- the second matches any character that is neither a digit nor whitespace. It matches `x`, but not `8`

* * *

### Modifications

#### Inverse
To [[inverse|dendron://thoughts-on/math.function.inverse]] the match, we just need to uppercase the character
- ex. `\s` matches a whitespace character, `\S` matches a non-whitespace char.
- ex. `[^0-9a-z]` matches any character that isn't numeric or lowercase

##### Match a word surrounded by whitespace
`\sthe\s`
