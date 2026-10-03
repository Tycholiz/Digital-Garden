
Regex is the art of defining a string format. This format could be as simple as a literal string (e.g. the word "email"), or it could be as complex as the pattern that an email address follows.

### The Eager Regex engine
Regex is eager, meaning it will always return the leftmost match even if there is a better match to be found later.
- put another way, the engine is “eager” to report a match.
- ex. if we were looking for the word `cat` in the sentence "He captured a catfish for his cat.", it would match the substring "cat" of the whole word "catfish", even though there is a single word "cat" at the end of the sentence. This shows how the regex engine returns the leftmost match; not the best match.

Imagine we used an alternation to match one of multiple strings: `Get|GetValue|Set|SetValue`. Because

1. The engine walks through the regex, attempting to match the next token (ie. character or character class) in the regex to the next character.
2. If a match is found, the engine advances through the regex and the subject string.
3. If a token fails to match, the engine backtracks to a previous position in the regex and the subject string where it can try a different path through the regex.

A regex engine always returns the leftmost match, even if a “better” match could be found later:
1. When applying a regex to a string, the engine starts at the first character of the string.
2. It tries all possible permutations of the regular expression at the first character.
3. Only if all possibilities have been tried and found to fail, does the engine continue with the second character in the text.
4. Again, it tries all possible permutations of the regex, in exactly the same order. The result is that the regex engine returns the leftmost match.

[source](https://www.regular-expressions.info/engine.html)

### Implementing Business Logic in Regex
Consider that Regex can be used to implement business logic. For instance, consider the following Regex:
```
[0-9]{1,2}(\.\d+)? MB
```

This Regex will match `25 MB`, `3 MB`, `7.45 MB`, but not `100 MB`. By using `{1,2}`, we've limited our search to matches that are less than 100 MB.

* * *

### Match vs pattern
- `pattern` - the regex text that we type into the search field
- `match` - any text in the document that is highlighted as a result of the search

* * *

While there are many implementations of regex that differ sometimes slightly, there are basically only two kinds of regular expression engines: text-directed engines, and regex-directed engines, with nearly all modern regex flavors are based on regex-directed engines
- certain very useful features, such as lazy quantifiers, backreferences, atomic grouping, possessive quantifiers etc., can only be implemented in regex-directed engines.

## Different flavors of Regex
The "normal" regex is Perl-Compatible Regex (PCRE)
- may be contrasted with Basic Regular Expressions (BRE)
- Vim Regex largely follows BRE

The main difference between the two is that BRE tends to treat more characters as literals - an "a" is an "a", but also a "(" is a "(", not a special character - and so involves more backslashes to give them "special" meaning.

#### BRE
- `\V` Verbatim Switch - only `\` has special meaning (ie. very nomagic)
  - prevent regex symbols from taking over - `/\Va.k.a` (equivalent to `/a\.k\.a\.`)
  - in regex, `.` means "match any character". making a verbatim search removes that functionality
  - when we use `\V`, it means for the following search, only `\` will have a special meaning
- `\v` Literal Switch - all characters (except word characters (**[a-zA-Z_]**)) have special meaning (ie. very magic)
- delimit words - since the word "the" appears in "these", if we wanted to just search for the word "the", we can use delimiters: `/\v<the>`

- ignore case - `/search\c`
- enforce case - `/search\C`

## Terms
- **magic** means we don't have to escape a character for it to take on its special meaning. 
- **nomagic** means we have to escape, because otherwise the character will be taken literally
  - *ex.* with magic, `.` will mean "stand in for any character". with nomagic, it will literally mean "match the `.` character"
  - this verbiage makes sense, because with magic, a lot of cool stuff is happening, but we have no idea how it's being done. Without magic, it's just looking for a character

# Useful Regex Patterns
`'\{\s.*ListView.*from\s.react-native.'` - search for a non-default exported module from a specific module
