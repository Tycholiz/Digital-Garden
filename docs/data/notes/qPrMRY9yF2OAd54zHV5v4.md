
### Syntax highlighting
Vim uses syntax definitions to highlight source code.
- Syntax definitions simply declare where a function name starts, which pieces are commented out and what are keywords.
- To color them, Vim uses colorschemes. You can load custom color schemes by placing them in .vim/colors, then load them using the colorscheme command

### Pasting
when you paste text into your terminal-based Vim with a right mouse click, Vim cannot know it is coming from a paste. To Vim, it looks like text entered by someone who can type incredibly fast :) Since Vim thinks this is regular key strokes, it applies all auto-indenting and auto-expansion of defined abbreviations to the input, resulting in often cascading indents of paragraphs.

There is an easy option to prevent this, however. You can temporarily switch to “paste mode”, simply by setting the following option:
`set pastetoggle=<F2>`

Then, when in insert mode, ready to paste, if you press `<F2>`, Vim will switch to paste mode, disabling all kinds of smartness and just pasting a whole buffer of text. Then, you can disable paste mode again with another press of `<F2>`. Nice and simple.
