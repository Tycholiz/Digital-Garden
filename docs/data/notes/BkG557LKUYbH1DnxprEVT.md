
- C is designed to interface with the output of the computer hardware. Therefore, it is a natural language to use when building operating systems. When you write code in C, you can better intuit what the resulting assembly language will look like. C effectively enables you to write code from a hardware-first perspective.

In C, if a function signature doesn’t specify any argument, it means that the function can be called with any number of parameters or without any parameters.

- `main` is a function just like any other, and thus returns a value to the environment that executed the c program to begin with.

# Header Files
- purpose is to share functions and macros across source files.

* * *

## Aspects
### Const
- a *constant expression* involves only constants, and is therefore evaluated during compilation 
- an `emun` is a type of constant. By default, the first element has value 0, the second has value 1, and so on.

## Operators
### Token-pasting operator (##)
- if 1+1=2, then 1##1=11

## Concepts

* * *

### Symbolic Constants (`#define`)
- `#define` creates a macro
- Any constant defined in this manner will find all occurrences and replace it with the corresponding value *before* compilation 
	- This contrasts with variables, in that data is actually stored inside of them (while macros are more like aliases) 
- Symbolic Constants are valauble for defining magic numbers

* * *

## Debugging
Errors involving “token” almost always mean that you either missed a semicolon or your {}s, ()s, or []s aren’t matched.
		
* * *

### Indirection
- The act of referencing something using a name, rather than using the value itself.
- A common form of indirection is when we manipulate a value via its memory address
	- ex. accessing a variable by using a pointer.
- aka Dereferencing

### Handle vs Pointer vs Reference
- While a pointer contains the address of the item it refers to, a handle is an opaque reference to an object. 
- The type of the handle is unrelated to the element referenced
- using handles adds an extra layer of indirection, meaning that we can change the details at this level without breaking the program. The same couldn't be said for a pointer. 
- A pointer is the combination of an address in memory and the type of the object that resides in that memory location
