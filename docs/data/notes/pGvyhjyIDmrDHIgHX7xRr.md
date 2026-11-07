
### The problem with building isolated components using CSS+HTML
A fundamental problem of building isolated components with HTML+CSS is that the CSS `display` property sets both an element's inner layout (how it lays out its children) and its outer layout (how it is laid inside it parent).
- ex. you have to make an element `display: inline-flex` to make it an inline flexbox. You can't just make it `flex` and have the parent decide whether it is inline, a block or whatever.

The consequence of this is that a parent component cannot properly layout a child component without knowing its internal layout structure. And if it doesn't don't know the child's layout (if it's dynamically supplied for example) then it can't safely do anything with it. So long as this limitation exists, there will never be true layout encapsulation on the web platform.

* * *

## Element Types: the `display` property
### Block-level elements
Occupies the entire horizontal space of its parent element, and vertical space equal to the height of its contents, thereby creating a "block".
Browsers display the block-level element with a newline both before and after the element. 
- You can visualize them as a stack of boxes.

The block-level element

#### Examples
`<div>`, `<p>`, `<h1>`, `<ul>`

### Inline elements
Only occupy the space bounded by the tags defining the element.
Does not start on a new line, and only takes up the width it needs.
Does not break the flow of the content.
You can't put block elements inside inline elements.
The block-direction margin is ignored entirely
The padding on inline elements doesn’t affect the height of the line of text
- ex. `padding-top` on an `<a>` tag won't cause the height of the line of text to change

All inline elements (not just text) can be laid out by a parent with the `text-align` property
- also with `vertical-align`, which would be the inverse property, manipulating upon the vertical axis.

#### Inline-block elements
Able to have width/height values set.
While `transform` property can't be used on inline elements, it can be used on inline-block.

Be careful with inline-block elements. The components you build with them may look fine, but the quirks may cause them not to place nicely with their parents:
![](/assets/images/2021-11-02-10-53-16.png) 

#### Inline-flex
Virtually the same as `display: flex`, except the flex container itself is inline, instead of block.
