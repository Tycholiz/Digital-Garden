
In its simplest form, Portable Text is an array of objects of `type` with an array of `children`

Portable Text is an agnostic abstraction of Rich Text.
It can be serialized into any markup language
- ex. HTML, Markdown, XML

Portable Text is built on the idea of rich text as an array of blocks, themselves arrays of children `<span>`s.
- Each block can have a style and a set of mark definitions


```json
{
  "style": "h1",
  "_type": "block",
  "children": [
    {
      "_type": "span",
      "text": "This is a heading"
    }
  ]
}
```

### Block
A section of text, such as paragraph or heading

Includes:
- `children`
- `_type`
- `style`
- `markDefs`
- `listItem`
- `level`

- includes all styling that is part of the section of text
    - ex. if a paragraph has 2 italicized words in it, then it will result in 5 objects in the `children` array, but it will be part of the same block.

#### `children`
The elements of the `children` array are sections of a block. They maintain the styling of the rich text. For instance, if the following were Rich Text, then we'd end up with a `children` array of length 5:
```md
I have to tell *you* what is happening! In fact, it was my **uncle** who shot at us.
```

It is split up like this;
```js
const children = [
    { text: "I have to tell ", marks: []},
    { text: "you", marks: ['em']},
    { text: "what is happening! In fact, it was my ", marks: []},
    { text: "uncle", marks: ['strong']},
    { text: " who shot at us", marks: []},
]
```

#### `_type`
All blocks must be of a type. The type makes it possible for a serializer to parse the contents of the block.

#### `style`
Style typically describes a visual property for the whole block (ie. all children).

#### `markDefs`
Mark definitions is an array of objects with a key, type and some data

Mark definitions describe data structures distributed on the children.
- Mark definitions are tied to spans in `children` by adding the referring `_key` in the child's `marks` array.
```json
{
  "markDefs": [
    {
      "_key": "some-random-key",
      "_type": "link",
      "href": "https://portabletext.org"
    },
    {
      "_key": "some-other-random-key",
      "_type": "comment",
      "text": "Change to https://",
      "author": "some-author-id"
    }
  ]
}
```

* * *

### Custom blocks
Custom blocks is typically images, code blocks, tables, video embeds, or any data structure. 
- Custom blocks should be given a `_type`.
```json
{
  "_type": "image",
  "asset": {
    "_ref": "some-asset-reference"
  }
}
```
```json
{
  "_type": "code",
  "language": "javascript",
  "code": "console.log(\"hello world\");"
}
```

* * *

## Full example
```json
[{
    "_type": "block",
    "_key": "3628734dd519",
    "style": "normal",
    "markDefs": [{
        "_type": "link",
        "_key": "e556761904ba",
        "href": "https://www.portabletext.org"
    }],
    "children": [{
            "_type": "span",
            "_key": "3628734dd5190",
            "text": "This is a paragraph with a ",
            "marks": []
        },
        {
            "_type": "span",
            "_key": "3628734dd5191",
            "text": "link",
            "marks": [
                "e556761904ba"
            ]
        },
        {
            "_type": "span",
            "_key": "3628734dd5192",
            "text": ".",
            "marks": []
        }
    ]
}]
```