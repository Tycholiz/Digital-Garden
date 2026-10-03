
## Overview
A fundamental concept in TS is the language's ability to describe the shape of javascript objects at the type level.
- One example of the implementation of this concept is the idea of **Declaration Merging**

In TS, a declaration creates entities in at least one of three groups: namespace, type, or value.
- Namespace-creating declarations create a namespace, which contains names that are accessed using a dotted notation.
- Type-creating declarations do just that: they create a type that is visible with the declared shape and bound to the given name.
- Value-creating declarations create values that are visible in the output JavaScript.
![](/assets/images/2021-04-04-13-05-32.png)

## Interface merging
the merge mechanically joins the members of both declarations into a single interface with the same name.
```ts
interface Box {
  height: number;
  width: number;
}
interface Box {
  scale: number;
}
let box: Box = { height: 5, width: 6, scale: 10 };
```

If 2 objects have a function member with the same name but different signatures, then they will be [[overloaded|paradigm.oop.overloading]], and both functions will appear on the merged object:
```ts
interface Cloner {
  clone(animal: Animal): Animal;
}
interface Cloner {
  clone(animal: Sheep): Sheep;
}

// The two interfaces merged will create a single declaration:
interface Cloner {
  clone(animal: Animal): Animal;
  clone(animal: Sheep): Sheep;
}
