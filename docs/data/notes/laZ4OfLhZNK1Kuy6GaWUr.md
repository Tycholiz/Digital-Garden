
### What happens when we run an executable?
1. as we type the characters `./hello` into the shell program, each letter is read into a register (in the CPU), and then stores it in memory
2. When we hit `<enter>`, the shell loads the executable `hello` file by executing a sequence of instructions that copies the code and data in the `hello` object file from disk to main memory (included is the string of characters `"hello, world\n"`)
3. the data then travels directly from disk to main memory, without passing through the processor (a technique known as *direct memory access*)
5. Once the code and data of the `hello` file are loaded into memory, the processor begins executing machine-language instructions in the `hello` program's `main` function.
	- These instructions copy the bytes in the `"hello, world\n"` string from memory to the register file (CPU), and from there to the display device (monitor).
