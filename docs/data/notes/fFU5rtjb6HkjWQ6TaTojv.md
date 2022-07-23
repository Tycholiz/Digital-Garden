
`is` is a keyword that is used in compile-time to tell the developers the code will have a chance to have a runtime error.
- `is` is a type guard that lets us model the behaviour that "this variable will be of *this* type within any guarded blocks of code"

```ts
// we say "test is string", meaning that inside any blocks that are guarded by this function, the value must be a string
function isString(test: any): test is string{
    return typeof test === "string";
}

function example(foo: any){
    // because of the "test is string" part above, we give a guarantee to typescript that "if isString" is returning true, then we guarantee that the foo value will be a string
    if(isString(foo)){
        console.log("it is a string" + foo);
        console.log(foo.length); // string function
    }
}
example("hello world");
```

Another example:
```ts
export function isBook(x: Item): x is Book {
  return x.itemType === 'book'
}

// imagine we have an array of objects with an `itemType` field
const filtered = items.find(isBook)
```

## E Resources
https://stackoverflow.com/questions/40081332/what-does-the-is-keyword-do-in-typescript
