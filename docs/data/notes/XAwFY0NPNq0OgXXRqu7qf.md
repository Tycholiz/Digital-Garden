
An opaque type is a type that "wraps" lower-level types, and is often used when either the underlying implementation is complex, or the user simply does not need to know about the inner workings
- ex. there is a type in Swift called `CFString`. It is an opaque type that provides a series of methods that allow for string manipulation and string conversion. For example, we have a `.length` and `.indexOf` methods. The implementation details of these methods is unimportant to the user of this type, so they have been hidden. 
	- This is the essence of an opaque type, in that a native type has had some extra functionality added to it by being "wrapped".

An "opaque type" is a type where you don't have a full definition for the struct (or class in the case of C++)
- In C, you can tell the compiler that a type will be defined later by using a forward declaration:
```c
// forward declaration of struct in C and C++ 
struct Foo;
```
here, the compiler only has enough info to be able to declare pointers to `Foo`, which is sometimes all we need to do
- Allows library and framework creators to hide implementation details, allowing the users of that library to call helper functions to create, change or destroy instances of a forward declared `struct` (also `class` in C++) 
