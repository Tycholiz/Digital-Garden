
# Styles
- apply styles to things like text to be able to reuse them easily
    - when selecting text, it is under the `Appearance` headline

# Layout
## Smart Layout
- Create a symbol that when modified (ex. text), will retain its ratio
    - ex. make a button, group it, then make into a symbol. On the naming
	window, select "horizontal align". Now, when you replace the text on
	that button, the horizontal size will automatically adjust
- Use smart layout any time you create a list of horizontal text (ex. a navbar,
    or tabs).
    - Each item should be a symbol that has a smart layout of "horizontal
	align".
    - The whole navbar itself should also be a symbol with "horizontal align",
	making the spacing between the items to be consistent even with changes
	to the text.
    - On the sidebar, you can choose to override styles in order to remove an
	item from the list. Each item will shift horizontally to take its place.

# Reusing
- Mainly done through `Layer Styles`, `Text styles` and `Symbols`

## Symbols
- similar to the concept of components in programming
- ex. create a button from scratch, similar to grouping them together, we can
    make them into a symbol to be reused throughout the design
- to make edits to the master symbol, just click on any symbol to be transported
    to the master symbol's canvas.
- [guide to reusing colors](https://www.reddit.com/r/sketchapp/comments/gwft1f/if_you_could_just_edit_these_and_for_the_change/)

### Nested Symbols
- ability to override the symbols
- Imagine we have a navbar and want to create an active/inactive state for each
    button.
- Create the whole navbar as one symbol, then when editing the master navbar symbol, click
    into each button and convert it to a symbol, naming it `nav/icon{1,2,3}/off`
- Duplicate each icon and make the ON state by changing color, then renaming to
    `nav/icon{1,2,3}/on`
- This allows us to easily insert each state of a symbol easily.
- We can quickly swap in on/off states now by clicking on the symbol (from the
    artboard) and overriding the styles on the sidebar
- If we want to swap the icon for another, we can just go to the navbar master
    symbol and modify each nested symbol (the icon), and it will be updated
    across all instances of the navbar symbol 
    
#### Thought experiment
Let’s say that a project requires 3 buttons in 3 different colours. To use only one symbol for all of them at once, we need to be able to change the colour of the button without detaching it from a symbol.
When you put a symbol inside another, you can swap the one inside with any other available symbol of the same size. In Sketch, this function is named nested overrides. This way, if you have multiple colour swatches saved as symbols, you can easily replace one swatch with another.

## Overriding symbols
- Say we have a vertical scrollable list of airbnb houses for rent, each with common styles. What we can
    do is create 1, make it a symbol, then insert that symbol into the document.
    On the sidebar, you can see all of the overrides that are available
    (adjusting text, adjusting colors, images etc)

* * *

## Mask
- use to chop a hole out of the object (ex. picture) in the shape of the
    overlying object.
    - take a square and hover over a picture, then click 'mask', and you have an
	imagine in the shape of the square.
### Alpha mask
- created when using opacity with gradients
- allows us to place a rectangle with a gradient at 0%-100% over text, and hit
    'mask' to impose that gradient on the text.
- the object on top will apply its gradient onto the object below

# Shortcuts
- `cmd+1`/`cmd+2` - zoom in/out on object
- `shift+cmd+l` - lock an object from movement
- `shift+cmd+h` - hide an object 
- `cmd+[`/`cmd+]` - send layer back/fwd
    - `cmd+opt+[]` - send to bottom/top
