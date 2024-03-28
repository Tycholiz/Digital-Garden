
`&` returns the memory address of the object
- called "address of" operator
	- Therefore, this gets applied to the value to get the memory address
- ie. `p = &c;`

`*` either: 
1. declares a pointer variable, or
	- ex. `int *p;`
2. dereferences an existing pointer (indirection through a pointer)
- called "indirection" or "dereferencing" operator
	- Therefore, this gets applied to the pointer to get the value in storage
- `int a = 5` then `int *ptr = &a;`
	- this means "locate" the address where `a` is stored, and assign its value to `ptr`
		- In other words, the value of `ptr` will now be the address where `a` is stored. 
- `*ptr = 8`
	- this means take the address of `ptr`, "locate" that address in memory, and set its value to 8.
- `int *ptr`
	- "`ptr` is a pointer that points to an object of type `int`", or simply: "`ptr` is a pointer to `int`."

If p points to the integer x, then *p can occur in any context where x could, so:
```
*p = *p + 10;
// is the same as
x = x + 10;
```

<!-- TODO: broken URL -->
![996035c6b28b5cf88b9156c920cc0d58.png](:/454c71a68a8845c99466685e0c038c4d)
- This picture shows how a pointer references a storage location, which holds a variable `c`. When we use the `&` operator, we are talking about the place where `c` is stored. When we use the `*` operator, we are talking about the variable `c` itself. 

- each pointer points to a specific data type, which is why we declare a pointer variable `int *p;`
	- The exception is "pointer to void", which can hold any kind of pointer, but cannot be dereferenced itself.
- C does not implicitly initialize storage duration of memory locations. Therefore, we should be careful that the address that the pointer points to is valid.
	- For this reason, some suggest initializing pointers to `NULL` (*null pointer*/*null reference*)
	- null pointer shown as `0x00000000`
- Like other languages, manipulating a function argument will have no effect on the original variable that was passed to the function. This is because when we pass an argument, a copy is made and we are merely mutating the copy. 
	- However, we are also able to call a function, passing in the variable's *address* as the argument `passByAddr(&x)`.
from within the function, if we change the value like so `*m = 14`. this changes the value at the address, so outside the function we'll notice that the value changed
