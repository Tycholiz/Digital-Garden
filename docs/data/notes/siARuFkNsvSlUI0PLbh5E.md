
RAM works by means of fast-switching pointers. This is analogous to how information retrieval from a database is so efficient. The engine doesn't really care what the data is it's retrieving— it only cares about getting it out.

### Main memory (RAM)
- the temporary storage device that holds both a program and the data it manipulates while the processor is executing the program.
- physically, main memory consists of a collection of DRAM chips.
- logically, memory is organized as a linear array of bytes, each with its own array index starting at 0.
- ex. Imagine every spot of RAM having its own address.
	- If your computer is 32 bit (4,294,967,296), then our computer can't talk to any address that is higher than that.
		- This much memory is commonly known as 4gb.

### Virtual RAM
Sometimes, the physical RAM runs short for actively running programs. When this happens, a block of space on the hard drive can be configured by the OS to pretend to be memory.

Of course, virtual RAM is a lot slower, so the more your computer is forced to rely on it, the less performant it will be
