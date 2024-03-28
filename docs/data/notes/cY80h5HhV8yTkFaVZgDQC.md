
Go up one level
`cmd+shift+up`

Open Lookup with current path pre-populated
`cmd+shift+down`

Cycle between note siblings
`cmd+shift+[`/`]`

Copy to clipboard a reference link of the highlighted header
`cmd+shift+r`

### Backlinks

Link to a note
- `[[code.git.cli]]` will link to a note of the same hierarchy
- we can generate a new note by entering a new hierarchy within the braces and hitting `F12`
- we can alias a backlink like this `[[git cli|code.git.cli]]`
- all backlinks of current note can be seen on sidebar

Embed a note/section of note in another note
- You can create a note reference by using CMD+SHIFT+R while inside a note, then paste that reference into another note. This creates an embedded note in the current note.
	- Instead of embedding a whole note, we can embed just what's within a header, by highlighting the head before using CMD+SHIFT+R
		- If we want to retain our original header, remove the `,1` in the reference

Link to particular section of a note
- You can use `CMD+SHIFT+C`
	- can do it for the whole note, or just a highlighted section

Extracting out text to put into a lower-level note
1. select text to extract
2. cmd+shift+s
3. turn "scratch note" off, and "selection extract" on
4. rename file
5. cmd+shift+r from the extracted file
6. paste into the original file to create the ref to the lower-level file

[Link ranges](https://wiki.dendron.so/notes/f1af56bb-db27-47ae-8406-61a98de6c78c/)


## Navigating
go up one level
- cmd+shift+up

## Searching
### Scoping search to a sub-tree
1. Open vscode advanced search with `<Cmd+shift+f>`
2. Input search term
2. In "files to include", include as many hierarchies (separated by `.`), followed by `**` at the end
    - ex. Search within `postgres` domain - `postgres**`
    - ex. `graphql.operators**`