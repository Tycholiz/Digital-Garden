
### Memory Hierarchy
- Because of physical laws, larger storage devices are slower than smaller storage devices.
	- ex. a disk drive on a typical system might be 100 times larger than the main memory, but might take 10,000,000x longer to read a word from disk than from memory. Similarly a typical register file (CPU) stores only a few hundred bytes of information, as opposed to millions of bytes in main memory. However, the processor can read data from the register file almost 100x faster than from memory.
- The main idea of memory hierarchy is that the storage at one level serves as a cache for storage at the next lower level

### Memory Leak
in languages without garbage collection, memory leaks occur when we fail to free up memory (deallocate) after a pointer no-longer serves its purpose (such as when we never reference that variable again until end of execution)

### Memory Cell
The memory cell is the fundamental building block of memory.
The memory cell itself is an electronic circuit that stores a single bit of binary information (ie. 0 or 1)
- This is achieved by sending either a high voltage signal (1) or low signal (0)
