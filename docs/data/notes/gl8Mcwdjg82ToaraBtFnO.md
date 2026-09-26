
## Constructor
A constructor's purpose is to allocate you an object and then return immediately.

## Class-related Keywords
### `super`
The `super` keyword is used to access and call functions on an object's parent.
- ex. If we have `Dog` extend from `Mammal`, then calling `super.get` in `Dog` will call the `get` method of the `Mammal` class.

spec: When used in a constructor, calling `super` calls the `constructor` method of the parent.
- the `super` keyword appears alone and must be used before the `this` keyword is used.

Note that a rectangle is more generic than a square. Therefore, we can create a `Square` class that extends a `Rectangle` class, and simplify the interface of the `Square`.
```ts
class Rectangle {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
}
class Square extends Rectangle {
    constructor(length) {
        super(length)
    }
}
```

We can call `super` on methods that have the `static` modifier.
