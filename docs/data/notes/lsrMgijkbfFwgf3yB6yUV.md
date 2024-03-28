
An interface fulfills the principle of "duck typing"

```ts
interface inputObjectI = {
	label: string
	optionalVal: string?
	readonly readOnlyVal: number // value is immutable post-declaration
}
```
Once we define the interface, it can then be used as a type.

Function interfaces can also be created
- ex. we have a fn with 2 params of type `String` and `String`, and the function returns `Boolean`
```ts
interface search {
	(source: string, substring: string): boolean
}
```

* * *

## Different perspectives of Interface
The purpose of an interface changes depending on if we are looking at it from an Object-Oriented perspective or a Function perspective.

### Interfaces in Object-Oriented Programming
In OOP context, an interface defines a list of methods that a class needs to implement in order to conform to the interface.
- often used to fulfill the Dependency Inversion Principle from SOLID design principles.
- The dependency inversion principles says that it’s much better to have interfaces than concrete classes as dependencies.

In general, the OOP part of interfaces is their ability to express some *required behavior*, as opposed to *data* in the functional paradigm.

### Interfaces in Functional Programming
In functional programming context, interfaces are used not to enumerate methods of an object, but to describe the shape of the data contained by the object.
- In this context, generic interfaces are used to describe a data shape when you don’t know or care about the exact type of some properties of the interface. It often makes sense when the data types contain some value.
