
## Types
In the early days of C, pretty much everything was either an int or pointer (which is why int is the default type)

### Numbers
Prefixes
- A leading `0` on an integer constant means octal
- a leading `0x` means hexadecimal

Suffixes
- spec:an int is by default short, and has no suffix:
`int num = 1234;`
- but to make a long constant (double by default), we need to append the number with `L`:
`long num = 123456789L`
	- note: even if `long` is not specified, the compiler will know it is long and declare it as such 
- to make an unsigned const, append `U`
- to make a float const, append `F`
