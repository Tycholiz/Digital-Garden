
While Javascript already has `typeof` that can be used in an expression context, Typescript adds a `typeof` that can be used in a type context.
- that is, we can use it to refer to the type of a property/variable.
- it’s only legal to use `typeof` on identifiers (i.e. variable names) or their properties

Like `keyof`, `typeof` is used to create new types

```ts
let s = "hello";
let n: typeof s;
```

typeof can be used to conveniently express many patterns.
- imagine we want to grab the return type of function `f()` for usage elsewhere:
```ts
function f() {
  return { x: 10, y: 3 };
}
// note: ReturnType is a built-in, and gives us the return type of a given function
type P = ReturnType<typeof f>;
```
