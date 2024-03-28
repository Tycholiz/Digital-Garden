
### Command line Completion
When executing a command line command, use `<C-d>` to see suggestions based on the text you've already typed
- doing this will give us a list of all the commands containing the string we've already typed
- ex. `:help CTRL-W`, then type `<C-d>` to see all commands with `CTRL-W`
Alternatively we can `<TAB>` to get the completion window

* * *

## Executing Shell commands
### Running commands without using output
Precede any shell command with `!` in the command prompt to have it interpreted as a shell command.

### Backtick expansion
If we wrap text in backticks, vim will run the enclosed command using our configured shell, and use its stdout as the argument for the given vim command
    `` :args `cat files.txt` ``
- if files.txt is just a list of files, then vim will add all those files to the argument list

## Getting pwd/filename
- `%` refers to filepath of current buffer
    - if our current buffer was 3 levels deeper than our pwd, we could open a sibling (same level) file with `:e path/to/file`, or we could just say `:e %:h<Tab>`, which would auto complete the folder of the current buffer
        - `:h` here removes the filename from the path
        - `%:h` has been aliased to `%%`

## Functions
we can call functions like so:
- `:call MyFunction()`
