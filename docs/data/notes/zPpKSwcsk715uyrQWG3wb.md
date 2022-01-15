
One of the keys to using Vim effectively is trying to edit by text object

# Text object 
- a defined region of text that is defined by the motions, and modifier of that motion (eg. `vaw`, or `v2aw`)
    - The definition of the text's scope depends on what vim commands we are using
        - ex. When we say `vi"` we are defining a text object, whose definition is "everything inside `""`". Text objects define regions of text by some sort of structure.
- text objects are good because we don't have to worry about where in the word the cursor is located. as a result, they work well with `.` 

## Types
- Word (`w`)
- Sentence (`s`)

## Extended Text Objects (Plugins)
### CamelCaseMotion
Defines a text object that is a single word in a larger camel-case word
- Delete inside the camel-case word
    - `di,w`

### VimTextObject
Defines a text object that is a function argument/parameter
- Delete inside the argument
    - `dia`

# UE Resources
[info](https://blog.carbonfive.com/2011/10/17/vim-text-objects-the-definitive-guide/)
