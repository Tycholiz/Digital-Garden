
## Block Formatting Context (BFC)
A BFC is a part of a visual CSS rendering of a web page. It's the region in which the layout of block boxes occurs

A BFC is basically a mini-layout within the main layout.
- This means, for instance, that floated elements will be contained within (achieved with `overflow: auto` on container).

BFCs prevent margin collapsing

Created by (non-exhaustive):
- `<html>` tag
- elements with `position` equal to `absolute`/`fixed`
- elements with `display` equal to `inline-block`
- Block elements where `overflow` has a value other than `visible` and `clip`.
- Flex items, provided they are not flex/grid containers themselves
- Grid items, provided they are not flex/grid containers themselves
