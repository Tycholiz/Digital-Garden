
In C, arrays can be thought of as pointers to consecutive areas of memory
- elements in an array are stored in physically adjacent blocks of memory 
	- spec: This is why a pointer pointing to an array only points to the first element— because we can essentially keep checking "is the next memory block part of that array?"
- the name of an array is a synonym for the memory location of the first element of that array. 
	- therefore, the assignment `p = &x[0]` is identical to `p = x`
	- also, what follows from this is that a reference to `x[i]` can also be written as `*(x+i)`
		- We can understand this as "`x[5]` is the address of the 5th element beyond `x`".
- When an array name is passed to a function, what is passed is the location of the initial element. Within the called function, the argument is a local variable, and so an array name parameter is a pointer (a variable containing an address)
- the syntax for accessing arrays is identical for that which can be used to dereference pointers
- `array[i]` is equivalent to `*(array + i)`. 
	- this fact shows that arrays are pointers to consecutive blocks of memory
- An array can be initialized with values like so: `int days[] = { 1, 2, 3 }` 
	- If the size of the array is omitted in initialization, the compiler will count the number of elements 
