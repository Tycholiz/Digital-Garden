
The pipe (`|`) in a union can be properly thought of as "or", since it means that "this variable can be of one type or the other"

A Union is a way to achieve type composition (as are [[intersections|ts.type.intersection]])
- composition being a way to create new types from existing types

The rule for union types is that we only allow an operation if it would be legal to do on each member of the union

```ts
let ambiguouslyEmptyAlice: Person | null | undefined;
```

Take the following union:
```ts
interface Foo {
    foo: string;
    xyz: string;
}

interface Bar {
    bar: string;
    xyz: string;
}

const sayHello = (obj: Foo | Bar) => { /* ... */ };
```
Here, we are saying "`Foo | Bar` is a type that has either all required properties of `Foo` OR all required properties of `Bar`."
- In the absence of a guard, we'd only be able to access `obj.xyz`, since that is the only property that exists on either type.


Union type is very often used with either null or undefined.
```ts
const sayHello = (name: string | undefined) => { /* ... */ };
```

### Relation to Unions in statistics
When we consider [[unions|math.set-theory.union]] in a statistical sense, it may seem counter-intuitive how Unions and Intersections are implemented in Typescript. 
- If have have a union type `string | number`, it means the type has to be either string or number.
- If we have an intersection type, it means the type must have both.

The lack of clarity lies in the perspective we are taking. We are meant to take the perspective of what is accepted as the union type. 
- we say that the set of things that can be set to `string | number` is the union of the string set and the number set.
    - ex. imagine there was a big list of all strings and all numbers that were loaded into memory. The union of `string | number` would be the set of all of them
- we say that the set of things that can be set to `string & number` is zero, because there is zero overlap between `string & number`.

Put another way, the incorrect way to see it is: "a union is a set operator on the field". The correct way is "a union is a set operator of the sets of what each type represents."
