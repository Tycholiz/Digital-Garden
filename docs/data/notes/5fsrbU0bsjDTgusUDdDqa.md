
## Lookaheads/Lookbehinds
- while the boundaries of a match normally correspond with the start and end of a pattern, we can use `\zs` and `\ze` to crop the beginning and end of a match, making the new match a subset of the pattern
  - ex. search for matches of "eagle", but only when it follows the word "bald"
    - `/bald \zseagle`
In this example, "bald eagle" still forms part of the pattern, but only the word "eagle" is matched
  - ex. search for everything inside quotes, without the quotes themselves
    - `/\v"\zs[^"]+\ze"`

Lookarounds are zero-length assertions, meaning they don't take place on a character of the string; they take place between characters.
- the difference between [[quantifiers|regex.quantifiers]] zero-length nature is that lookaround actually matches characters, but then gives up the match, returning only the result: match or no match.
  - ie. They do not consume characters in the string, but only assert whether a match is possible or not.
- Lookaround allows you to create regular expressions that are impossible to create without them, or that would get very longwinded without them.
