
FZF afts like an interactive filter. The responsibility of the program is not to get us the list of occurrences of our search pattern; that is the job of the search tool we use (eg. grep, ack, ag, find). FZF's only job is to take the list of occurrences as input, and perform the fuzzy searching logic on that list.
- FZF's default search program is `find`, but it can be changed with the `FZF_DEFAULT_COMMAND` env variable.
- Silver Searcher is good, because it respects the `.gitignore` and `.ignore` files.
