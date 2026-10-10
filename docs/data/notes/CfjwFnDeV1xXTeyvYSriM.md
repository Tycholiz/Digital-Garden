
A pointer is a variable that stores both a memory address and the type of data that resides at that memory location.
- Obtaining the value stored at that location is known as *dereferencing the pointer*
    - anal: The index within a textbook has page numbers which reference pages in the book. Dereferencing the pointer would be done by flipping to that page and reading the text.
	- the type of the pointer tells the compiler what operations can be performed through that pointer
- a pointer is a very thin abstraction built on top of a language's own addressing capabilities
- Each unit of memory in a system is assigned a unique address (memory location).
	- A pointer is an object that stores this address.
		- This is similar in concept to how we dont store images in a database, we just store its url location
	- Conceptually, the fact that memory is just stored in blocks makes memory itself a very large array.
	- arrays work a bit differently in that the variable points to the address (location) of the first character in the array 
- pointers are used partly because they are sometimes the only way to express a computation, and partly because they usually lead to more compact and efficient code than can be obtained in other ways
	- They are especially efficient when used in repetitive operations
- pointers are used for constructing *references*, which in turn are fundamental to constructing nearly all data structures
- using a pointer to retrieve the value that is stored at the relevant memory location is called **dereferencing the pointer**
- anal:if storage were a reference book, then the page number in the index would be the pointer, and flipping to that page and reading the text would be dereferencing that pointer.
- the format of a pointer is dependent on the underlying computer architecture (since pointer's pertain to physical memory locations)
- Pointers and arrays are closely related
	- Any operation that can be achieved by array subscripting can also be done with pointers
	- consider that when we declare an array `x` of length 10, we are reserving 10 contiguous memory cells. We can create a pointer that points to the first element with `ptr = &x[0];`. Now, we can copy the contents of that first element to `y` with `y = *ptr;`.
		- Since elements in an array are contiguous, by defintion `ptr+1` points to the second element in the array, and `*(ptr+1)` refers to the *contents* of `x[1]` 
	- ex. a char occupies a single byte, a short occupies 2 contiguous bytes, a long occupies 4 contiguous bytes, and so on.
		- Naturally, this means that contiguous groups of memory cells can be manipulated
- A pointer is a group of cells (often two or four) that can hold an address

### Example
Imagine we executed the following code:
```
int a = 5;
int *ptr = NULL;
```
assuming that `a` is stored at `0x8130` and `ptr` at `0x8134`, our memory would look like:
| Address | Value |
|---------|------------|
| 0x8130  | 0x00000005 |
| 0x8134  | 0x00000000 |

now this code is run:
```
ptr = &a;
```
And our memory looks like this:
| Address | Value |
|---------|------------|
| 0x8130  | 0x00000005 |
| 0x8134  | 0x00008130 |

now we can dereference `ptr`:
```
*ptr = 8;
```
And our computer takes the contents of `ptr` (0x00008130), locates the address, and assigns 8 to that location, yielding this memory:
| Address | Value |
|---------|------------|
| 0x8130  | 0x00000008 |
| 0x8134  | 0x00008130 |
