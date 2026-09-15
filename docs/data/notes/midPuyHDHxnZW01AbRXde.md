
Quantifiers help us achieve repetition
- they will match the whole character class; not just the character that was actually matched
	- ex. The regex `[0-9]+` can match 837 as well as 222.
	- If you want to repeat the matched character, rather than the class, you need to use [[backreferences|regex.backreferences]]
		- ex. `([0-9])\1+` matches 222 but not 837

Quantifiers can be applied to an entire group of character classes with `()`
- ex. `Nov(ember)?` matches Nov and November.

Match zero or 1 time: `?`
- identical to `{0,1}`

Match zero or more times: `*`
- identical to `{0,}`
- ex. match an HTML tag without any attributes: `<[A-Za-z][A-Za-z0-9]*> `

Match one or more times: `+`
- identical to `{1,}`

### Limiting repetition
Repeat the previous character class once (for a total of 2)
`hel{1}o` matches hello.

Repeat the previous character class between 0 and 3 times
`hel{0, 3}o` matches helo, hello, helllo...

Repeat the previous character class at most 3 times
`hel{,1}o` matches helo, hello

Repeat the previous character class at least 1 time
`hel{1,}o` matches hello, helllo...

### All repetition operators are greedy
Imagine we had the regex `Get(Value)?|Set(Value)?`. If we had a string "SetValue", we would match the whole string and not just Set, due to the fact that quantifiers are greedy.

Because repetition operators are greedy, they (may) have to backtrack to complete the full regex.
- Backtracking slows down the regex engine. This isn't noticeable if doing a simple text search in a file, but we start to notice a drain on CPU if we have a regex that we need to backtrack through on each iteration of a loop.

Most people new to regular expressions will attempt to use `<.+>`. They will be surprised when they test it on a string like: This is a `<EM>first</EM>` test. You might expect the regex to match `<EM>` and when continuing after that match, `</EM>`.

But it does not. The regex will match `<EM>first</EM>`. Obviously not what we wanted. The reason is that the plus is greedy. That is, the plus causes the regex engine to repeat the preceding token as often as possible. Only if that causes the entire regex to fail, will the regex engine backtrack. That is, it will go back to the plus, make it give up the last iteration, and proceed with the remainder of the regex.

Imagine we had a string `This is a <EM>first</EM> test.`. Not taking into account the greedy nature of repetition operators, a naive approach to match any HTML tag with regex `<.+>`. However, this matches the whole substring `<EM>first</EM>`.
1. we match the `<` literal
2. then we match any character 1+ times
3. when we arrive at the first `>` character, we are still evaluating for the `.`. Since `>` is a part of the `.` set (ie every character), it gets interpreted as such
4. by virtue of the `.`, we arrive at the end of the sentence, so we have `<EM>first</EM> test.`
5. now the engine realizes that it is at the end of the string, and it still have more things to match (ie. the `>` character), so it backtracks until it reaches that character in `</EM>`.
6. `<EM>first</EM>` is matched
	- because of the eagerness, the regex engine is eager to return a match. Therefore, it will not backtrack more than it has to. Once it has found a full match, it will return it. Because of the greediness, this is the leftmost longest match.

#### Turning greediness off
The operator can be turned lazy (ie. turn greediness off) with `?`.
- e.g. `+?`, `??`, `*?`
	- ex. in the string "Today is Feb 23rd, 2003", the regex `Feb 23(rd)?` will match "Feb 23rd", and the regex `Feb 23(rd)??` will match "Feb 23".
