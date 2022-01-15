
### Compiler
- unlike other languages, the C compiler does not look ahead to hoist definitions. It doesn't look backward or forward, nor does it scan the file multiple times to understand relationships. The compiler only scans forward in the file exactly once. Connecting function calls to function declarations is part of the linker's job, and is only done after the file is compiled down to raw assembly instructions.
	- This means that as the compiler scans forward through the file, the very first time the compiler encounters the name of a function, one of two things have to be the case: It either is seeing the function declaration itself, in which case the compiler knows exactly what the function is and what types it takes as arguments and what types it returns — or it's a call to the function, and the compiler has to guess how the function will eventually be declared.

#### Compilation process
we can get all intermediate files with `clang -Wall -save-temps filename.c -o filename`

1. Pre-processing
	- takes the source file and handles...
		- Removal of Comments
		- Expansion of Macros
		- Expansion of the included files
		- Conditional compilation
	- creates `filename.i`
	- At the end of the file, our source code is preserved
2. Compilation
	- takes the `filename.i` file and compiles it, creating assembly level instructions that assembler can understand
	- creates `filename.s`
	- compilation phase is useful because it provides a common output language for different compilers for different high-level languages
		- ex. compilers of C and Fortran produce the same assembly language.
3. Assembly
	- converts assembly language to machine level instructions
	- creates `filename.o`
		- produces a *relocatable object file*
4. Linking
	- all the linking of function calls with their definitions are done
	- at this phase, we take code that has already been compiled (like `printf`, which comes from the `printf.o` file), and merge it with our own `.o` file.
		- We can see the effect of this phase by running `size filename.o` and comparing it to `size filename`. We see that `filename` is much larger
		- produces a *executable object file*, which can be loaded into memory and executed by the system.

#### Different compilers
In C, it's possible that one compiler fails while another succeeds.
- In the gcc compiler, `main()` cannot return `void`, but Turbo C compiler allows this.
- To ensure we are writing proper C code, we have to look at the C Standard.
- Therefore, just because C code compiles doesn't mean it is up to C standard.  
