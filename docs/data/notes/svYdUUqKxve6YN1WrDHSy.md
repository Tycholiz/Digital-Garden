
### Variables
- `external` and `static` variables are guaranteed to be initialized to zero if not explicitly declared.
	- The value of these variables is determined conceptually before the program begins execution 
- automatic and register variables have undefined (i.e., garbage) initial values, if not explicitly declared.
	- The value of these variables is determined when the function or block is entered.

Static
- Decalaring a variable or fn *static* does 2 things:
1. it becomes scoped to wherever it was defined (scoped to source file, fn etc)
2. When static is declared inside a function, then that variable will live on between calls of the function, giving us permanent stoage. 
- Static variables are allocated memory in data segment, not stack segment
- Unless initialized with a value, static variables default at ‘0’

Register
- Declaring a variable with `register` advises the compiler that it will be heavily used, placing it in a machine register for quicker access
- Only a few variables in each function may be kept in registers, and only certain types are allowed.
	- Excess register declarations are harmless, however, since the word register is ignored for excess or disallowed declarations
		- The actual limit varies from machine to machine.
	- It is impossible to get the memory address of a register variable. 
