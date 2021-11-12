
A utility type is also known as a Type Constructor

Type constructor is a function (in the mathematical sense) that takes a type and returns a new type (based on the original type).
- Notice that a generic interface with only one type argument is exactly that

Utility types allow us to construct a new type, taking an initial type, and modifying it some way, determined by which utility type we use.

Think of a utility type as a function that takes in 1+ types, and returns a new type

## Examples
- `Partial<Type>` - construct a new type with all properties of `Type`, but those properties are all optional.
- `Required<Type>` - construct a new type with all properties of `Type`, but those properties are required. 
    - therefore opposite of Partial
- `Record<Keys, Value>` - contruct a new object type, where the keys are of type `Keys` and the values are of type `Value`
