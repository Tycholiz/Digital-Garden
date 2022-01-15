
### Char
- A char holds the ASCII value, rather than the character itself.
	- Therefore, by definition a `char` is just a small integer
	- We can leverage the fact that a char will evaluate to its ASCII value, by performing math on them
		- ex. we can test if something is a digit like with `if (val >= '0' && val < '9')`. Here, the `'0'` and `'9'` get converted to their ASCII values as the comparison is made. As it so happens, any ASCII value between 48 and 57 is a digit.
- a char is a single byte
