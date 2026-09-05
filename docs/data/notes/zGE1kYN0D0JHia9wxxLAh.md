
The `keyof` operator takes an object type and produces a union of its keys

Like `typeof`, `keyof` is used to create new types

For any type `T`, `keyof T` is the union of public property names of `T`
```ts
interface Person {
  age: number;
  name: string;
}

type PersonKeys = keyof Person; // "age" | "name"
```

The type `PersonKeys` here is the same as:
```ts
type PersonKeys = "age" | "name"
```

If the type uses a string or number as the index, then `keyof` will return those types instead:
```ts
interface Arrayish = {
    [n: number]: unknown
}
type A = keyof Arrayish
// A = number
```
