
### Static
The element goes with the normal document flow. `z-index` has no meaning here, and neither do `top`/`left`/`bottom`/`right`
This is default

### Relative 
The element goes with the normal document flow, and then is offset relative to itself (using `top`/`left`/`bottom`/`right`)
The element does not impact any other elements in the document
The space given to the component in the page layout is the same as if position were `static`

### Absolute
The element is removed from the normal document flow, and no space is created for the element in the page layout.
Instead, the element is positioned relative to it's closest positioned ancestor (ie. an ancestor with a position value that is not `static`)
- If there are no positioned ancestors, then the element is positioned relative to the initial containing block (ie. the parent of the `<html>` tag)

Creates a new stacking context as long as `z-index` is not set to `auto`

### Fixed
The element is removed from the normal document flow, and no space is created for the element in the page layout.
The element is positioned relative to the initial containing block (ie. the parent of the `<html>` tag)
If one of the element's ancestors has any of: `transform`, `perspective`, or `filter`, then that ancestor is used as the containing block.
Creates a new stacking context

### Sticky
The element goes with the normal document flow, and then is offset relative to 2 containing blocks: the nearest scrolling ancestor, and the nearest block-level ancestor.
