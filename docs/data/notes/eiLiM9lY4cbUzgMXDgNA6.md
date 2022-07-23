
## History Expansion
[source](https://www.thegeekstuff.com/2011/08/bash-history-expansion/)
- History expansion is performed immediately after a complete line is read, before the shell breaks it into words, and is performed on each line individually

Designators:
- Event Designators - refer to a particular command in history
	- start with `!`
- Word Designators - refer to a particular word of a history entry
	- often gets combined with Event Designators, which are demarcated by `:`
	- ex. `!cp:^` finds the most recent `cp` command in history and grabs the 1st argument
- Modifiers - modify result of the Event/Word Designator

Commands:
- `!ls` - execute most recent `ls` command
	- ex. if I ran `git log`, then `!git`, bash will say "oh ok, you want the last executed bash command", and will run `git log`
- `!?apache` - execute most recent command that contains the keyword `apache`.
	- `!%` will refer to the word that was matched by the previous `!?<pattern>` search
- `!-3` - execute 3rd most recent command
	- `!!` === `!-1` (most recent command)
- `!$` - reuse last part of the most recent command
	- ex. `less ~/myfile` then `vim !$` will run `vim ~/myfile`
- `!*` - reuse all parts of most recent command
- `^ls^cat^` - modify the pattern `ls` with `cat` in the previous command
- `!!:s/ls -l/cat/` - replace `ls -l` of previous command with `cat`
	- `!!:gs/...` for global substitution
- `!cp:^` - get 1st arg of the most recent `cp` command
- `!cp:$` - get last arg of the most recent `cp` command
- `!cp:2` - get 2nd arg of the most recent `cp` command
- `!cp:*` - get all args of the most recent `cp` command

* * *

## Tilde Expansion
### Tilde prefix
The tilde prefix includes all of the characters before the first slash `/`

`~`
`$HOME`
- in reality, this is shorthand for `~kyletycholiz` (the current user)

`~+`
`$PWD`

`~-`
`$OLDPWD`
- or, the directory we were in previous to our PWD
