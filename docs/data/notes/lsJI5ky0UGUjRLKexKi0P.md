
### Preprocessors (header of the file)
- contains C function declarations and macro definitions to be shared between several source files.
	- In essence, they are like importing modules.
- ex. `stdio.h` – Defines core input and output functions
- In C, all lines that start with # are processed by preprocessor which is a program invoked by the compiler
	- a preprocessor takes a C program and produces another C program without any `#`
- When we use `include` directive,  the contents of included header file (after preprocessing) are copied to the current file.
	- `<` and `>` instruct the preprocessor to look in the standard folder
	- `“` and `“` instruct the preprocessor to look into the current folder
- When we use `define` for a constant, the preprocessor produces a C program where the defined constant is searched and matching tokens are replaced with the given expression
	- ex. `#define max 100` — `max` is searched for in the program, and is replaced with `100`
