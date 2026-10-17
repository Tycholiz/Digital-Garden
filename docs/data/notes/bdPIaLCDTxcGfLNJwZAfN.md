
C does not have a native string type. Instead, the language uses arrays of char terminated with a null char (`'\0'`)
- Therefore, the following are equivalent:
```c
char pattern[] = "ould";
// and
char pattern[] = { ′o′, ′u′, ′l′, ′d′, ′\0′ };
```
- Therefore, the physical storage required is one more than the number of characters
	- The `\0` gets automatically appended for us when array is initialized. Therefore if we access the last element, we get back `\0`

There is a standard header `<string.h>` that gives us some string utilities

In C, arrays are of fixed length at the time they are declared. For this reason, it may make sense to declare an array with a length that is far more than you need, just so you know it can handle everything that gets thrown at it. 

### Single vs Double Quotes
In C, `'x'` is an integer representing the ASCII value, while `"x"` is a character array with 2 elements: `x` and `\0` 

## Initialization
- Character arrays can be initialized in different ways:
	- `char name[13] = "StudyTonight";`
	- `char name[] = "hello";` (spec:wide char array)
	- `char name[10] = {'L','e','s','s','o','n','s','\0'};`
- These character arrays can either be read-only or manipulatable
	- read-only: `char * str = "Hello";`
	- manipulatable: `char str[] = "Hello";`

## Format specifiers 
- `%s` - Take the next argument and print it as a **string**
- `%d` - Take the next argument and print it as an **int**

## printf (print formatted)
The compliment to `printf` is `scanf`, in the sense that print is stdout and scanf is stdin. 
- `scanf` terminates its input on the first white space it encounters. Tip: use "edit set conversion code `%[..]` " to get around this.
	- alternatively, use `gets()`, which will terminate once hitting `\n`

