
In C, we must declare a function prototype before calling the function so that the signature is known to the compiler 
- this is no different from declaring an int with `int e`
- The prototype looks identical to the function, expect the body is replaced by a semi-colon:

```c
char *do_something(char *dest, const char *src);
```	

This is not strictly necessary, but unless we declare it, the compiler is left to guess the signature based on how the function is called, and it is often wrong.
- if we are passing an argument that should remain constant within the function body, we can prepend `const` to the parameter: `int strlen(const char[]);`
