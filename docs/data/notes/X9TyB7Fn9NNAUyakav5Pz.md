
A value object is a small object that represents a simple [[general.terms.entity]]

Two value objects are considered equal when they have the same value. That is, they don't necessarily have to be the same object, since 2 different objects can have the same value.
- another way of saying this is "equality is not based on identity".

Consider that in [[C|c]], there is no concept of a string; only a [[character array|c.lang.types.array]]. Therefore, strings are abstractions on top of character arrays that serve 2 purposes:
1. they reduce complexity
2. they enforce ubiquitous language

This is precicely what Value Objects do for us. 

Value Objects are simply values that exist in our system, and they generally come from the ubiquitous language.
- generally, applications will use primitives like float for money, string for email... but in DDD, we shouldn't do this. Instead, we should be using value objects.
- Some say that primitives should not exist in our objects at all.

spec: value objects are made from primitives.

Value objects are among the building blocks of [[DDD|general.principles.DDD]].

### Examples of Value Objects
objects representing...
- money
- date range

### Features of Value Objects
- They are fungible
    - ex. $100 in a system can be replaced with any other $100 without consequence
- They are immutable.
    - ex. $100 in a system shouldn't be able to be changed to $50
    - this is required for the implicit contract that two value objects created equal, should remain equal.
    - It is also useful for value objects to be immutable, as client code cannot put the value object in an invalid state or introduce buggy behaviour after instantiation
- Rich in domain logic
- Auto-validating
    - the business logic is baked in. For instance, if we have our own `money` type, that is implemented in floats, the value object itself should implement the logic that only 2 decimal places can exist, negative values don't exist etc.

### Implementation
```java
public class StreetAddress
{
    public StreetAddress(string street, string city)
    {
        Street = street;
        City = city;
    }

    public final string Street { get; }
    public final string City { get; }
}
```
