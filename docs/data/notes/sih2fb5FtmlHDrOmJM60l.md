
TS takes a more parametric approach to generics 
- ie. the type is a variable being passed in just like the values, and the type is unknown while writing the function, which necessarily limits what you can do with it

We must pass in the type parameters (e.g. `<T>`), because otherwise Typescript wouldn't know if `T` is a type argument or an actual type.

It's critical to remember that everything in TS happens at compile-time. Therefore, against what your intuition might suggest, generic types are not checked when the function is called (ie. run-time). This is the beauty of Typescript, because every type must be known in advance, even though the code has not even been run yet. Therefore, we need to be strict in our typing, which means allowing for a given variable to be any type that it may be *depending* on the conditions happening during runtime.
- ex. consider that while an API is *expected* to return some data of a certain structure, that may not always be the case. Perhaps the network request fails, and the variable you are trying to set remains `undefined`. Therefore your type system must be smart enough to recognize this possibility. You must therefore build that possibility into the system.

## Generic constraints (ie. `extend`ing generics)
### Constraining type parameters by interfaces
Imagine we have a function that returns the length of the argument
```ts
function getLength<T>(arg: T): number {
    return arg.length
}
```

Written like this, only string and array would be able to work, and everything else would throw an error. To solve this, we can use an interface and extend the generic:
```ts
interface Lengthwise {
  length: number;
}
function getLength<T extends Lengthwise>(arg: T): number {
// ...
```
### Constraining type parameters by other type parameters
This generic constraint basically says "`Key` can only be a public property in `Type`".
- It has nothing to do with extending a type or inheritance, contrary to extending interfaces.
```ts
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key];
}

const person: Person = {
  age: 22,
  name: "Tobias",
};

// name is a property of person
// --> no error
const name = getProperty(person, "name");

// gender is not a property of person
// --> error
const gender = getProperty(person, "gender");
```

* * *

### Default generic parameter
In Javascript, we can set a default parameter if one isn't passed:
```js
function registerName(fullName = 'John Doe') {
```

In Typescript, we can set a default generic type, if the type isn't passed:
- this means that if `Data` type isn't passed (which must extend to `DataType`), then the generic will be `DataType`
```ts
export interface FormProps<Data extends DataType = DataType> { }
```

#### Example: Implementing HTML
- type `T` of the following signature defaults to a `div` element. If `create` is called with its first argument, then it will extend an `HTMLElement` type.
- type `U` defaults to an array of type `T`. If `create` is called with its second argument, `U` becomes the type of whatever was passed in
    - ex. if a `<p>` was passed as a child of the main element being created, then `U` = `HTMLElement`. If an array of `<p>` was passed, then `U` = `HTMLElement[]`
```ts
declare function create<
    T extends HTMLElement = HTMLDivElement, 
    U = T[]
>(element?: T, children?: U): Container<T, U>;
```

A generic parameter default follows the following rules:
- A type parameter is deemed optional if it has a default.
- Required type parameters must not follow optional type parameters.
- Default types for a type parameter must satisfy the constraint for the type parameter, if it exists.
- When specifying type arguments, you are only required to specify type arguments for the required type parameters. Unspecified type parameters will resolve to their default types.
- If a default type is specified and inference cannot chose a candidate, the default type is inferred.
- A class or interface declaration that merges with an existing class or interface declaration may introduce a default for an existing type parameter.
- A class or interface declaration that merges with an existing class or interface declaration may introduce a new type parameter as long as it specifies a default.

* * *

### Unbound Type Variable
Take the following example:
```ts
function foo<T>(): T {}
let x = foo();
// what type is x? the world may never know...
```

`foo` can use `T` inside the implementation, but it has to treat it as basically `unknown`, because it could be anything at all
- put another way, `T` is `unknown` during compile-time, and only known during run-time.
from the caller's perspective, if you haven't given it something to infer `T` from, it will just default to its constraint. And if there's no constraint, that means `unknown`

Here's some examples that help illustrate the issue:
```ts
declare function reduce <T, Acc>(inputArr: T[], reducerMethod: (accumulator: Acc, currVal: T) => Acc, initialValue?: Acc): Acc;

reduce(["string"], (acc, val) => acc.concat(val), [] as string[])
// reduce<string, string[]>, 😀
reduce(["string"], (acc, val) => acc.concat(val))
//                               ^^^
// Object is of type 'unknown'.
// reduce<string, unknown>, errors with a somewhat mysterious error 🤔
reduce<string, string[]>(["string"], (acc, val) => acc.concat(val));
// reduce<string, string[]>, compiles 😭
```
