
Typically, a `short int` is 16 bits (2 bytes), and `long int` 32 bits (4 bytes)
- therefore, an int is a 32-bit data type
	- this depends on the natural size of integers on the host machine
- in reality, 31 bits are available for the number, since 1 has to be reserved for the sign (+/-)
- Whenever a number is being assigned to an ‘int’ type variable, it is first converted to its binary representation then it is kept in memory at specific location.
