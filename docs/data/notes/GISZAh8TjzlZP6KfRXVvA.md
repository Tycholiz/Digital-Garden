
- using explicit margins breaks flexbox's ability to position things nicely, since it doesn't play nicely with the math that it's relying on
- when doing flexbox row don't use margin left/right, and when doing column, don't use margin top bottom, because it interferes with the math that flex is doing
    - try and use justify-content: space-between instead, and set flex: 0 49% for example, which will almost serve as our margin
 
note: all examples below assume `fd: row`. If `column`, then substitute width for height.

## Flex-Grow
- when there is available space after all boxes have taken up their room, `flex-grow` will allow the item to grow proportionally to the other item's `flex-grow` property.
    - therefore, all items having flex-grow: 1 is the same as all items having flex-grow: 100. Think of it as a proportional growth rate. 
    - an item with flex-grow: 3 will grow 3 times faster than an item with flex-grow: 1 
- a value of `0` means the item won't be resized during the size calculation to
    accommodate the flex container's full main axis size
- Imagine we use different flex-basis for each flex item. Even if we have `flex-grow: 1` on each item, they will still grow at a different rate, because of the basis at which they started.

### UE Resources
[flex grow article](https://css-tricks.com/flex-grow-is-weird/)

## Flex-Shrink
- The rate at which a flex item will shrink when the page is resized to a size smaller than the flex-containers most "comfortable" size.

## Flex-Basis
- initial main size of a flex item before any available space is distributed
    among the flex items (which is determined according to flex-shrink and
    flex-grow). 
- It will override any width property, unless left as `auto`
    - when set to `auto`, it checks for a width property (if fd: row). If none found, it uses the content inside the flex-item to determine its width.
    - `flex-basis` still obeys `min/max-width` settings
- In a sense, it is similar to `min-width`
    - (although `flex-shrink` determines how the flex item will behave when the
    size of the whole flexbox shrinks below initial size)
    - If we were to use `min-width` and shrink the browser size, the browser would make us scroll horizontally to see all of the content.

## Flex Shorthand
- by default, it is `flex: 0 1 auto` (?)
- `flex: 1 1 0` - ==flex-grow== ==flex-shrink== ==flex-basis==
    - also can be written `flex: `
