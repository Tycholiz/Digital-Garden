
An Intersection is a way to achieve type composition (as are [[unions|ts.type.union]])

Consider the following intersection:
```ts
const sayHello = (obj: Foo & Bar) => { /* ... */ };
```

Here, we are saying that `obj` must have *all* properties that are included on both `Foo` and `Bar`. 
