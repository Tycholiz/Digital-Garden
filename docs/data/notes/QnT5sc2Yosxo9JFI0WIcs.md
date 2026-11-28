
## What is an AST?
Think of an AST as a big nested object that holds metadata about the thing we are describing.
- ex. we could generate an AST of a single Javascript file. The AST would have a field called `body`, which is an array of parts: 
  - variable declaration 
  - expression statement 

### Use cases
- ASTs are commonly used in compilers (like the one found in [[js.v8]]) to parse the code we write, converting it into a tree structure that we can traverse programmatically.
- [[Linters|linter]] also make use of ASTs, recognizing certain parts of the code and applying rules that can be picked up by an IDE / command-line tool.
  - ex. The `no-unused-vars` looks at all `VariableDeclaration` types and ensures that the variable names are used at least once elsewhere in the code.
- Syntax highlighting is made possible by ASTs
  
## Example
Imagine we have an extremely short file `file.js`:
```js
const myname = 'jason'
console.log(myname)
```

The resulting AST of this file would be:
```json
{
  "type": "Program",
  "start": 0,
  "end": 43,
  "body": [
    {
      "type": "VariableDeclaration",
      "start": 0,
      "end": 22,
      "declarations": [
        {
          "type": "VariableDeclarator",
          "start": 6,
          "end": 22,
          "id": {
            "type": "Identifier",
            "start": 6,
            "end": 12,
            "name": "myname"
          },
          "init": {
            "type": "Literal",
            "start": 15,
            "end": 22,
            "value": "jason",
            "raw": "'jason'"
          }
        }
      ],
      "kind": "const"
    },
    {
      "type": "ExpressionStatement",
      "start": 23,
      "end": 42,
      "expression": {
        "type": "CallExpression",
        "start": 23,
        "end": 42,
        "callee": {
          "type": "MemberExpression",
          "start": 23,
          "end": 34,
          "object": {
            "type": "Identifier",
            "start": 23,
            "end": 30,
            "name": "console"
          },
          "property": {
            "type": "Identifier",
            "start": 31,
            "end": 34,
            "name": "log"
          },
          "computed": false,
          "optional": false
        },
        "arguments": [
          {
            "type": "Identifier",
            "start": 35,
            "end": 41,
            "name": "myname"
          }
        ],
        "optional": false
      }
    }
  ],
  "sourceType": "module"
}
```

# Tools
[AST Explorer tool](https://astexplorer.net/) —multiple language support
