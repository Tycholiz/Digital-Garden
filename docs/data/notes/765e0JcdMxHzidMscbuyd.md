
# Overview
The Vim documentation consists of two parts:
1. The User manual
   Task oriented explanations, from simple to complex.  Reads from start to
   end like a book.
2. The Reference manual
   Precise description of how everything in Vim works.

If you only have a generic keyword to search for, use `:helpgrep` and open the quickfix window:
```
:helpgrep quickfix
:copen
```

# How to...

## Navigate
Jump to a subject
- put cursor on a subject tag (blue text), and hit `<C-]>`
    - In fact, this works on any word
- return with `<C-o>`, or `<C-t>`

## Get specific help
we can prepend a binding with the mode to see what it does, and get help on it
- ex. `:help v_u` to get help on what `u` command does in visual mode.
- ex. `:help CTRL-W_CTRL-I`
- ex. `:help i_CTRL-G_<Down>`
- ex. `:help i^W`

prefixes:
visual mode - `v_`
insert mode - `i_`,
Ex mode - `:`
- normal mode has no prefix


# UE Resources
[Guide to using vim help](https://vi.stackexchange.com/questions/2136/how-do-i-navigate-to-topics-in-vims-documentation)
