
When a software program uses memory there are two regions of memory they use, (apart from the space used to load the bytecode), [[Stack|memory.stack]] and [[Heap|memory.heap]] memory.

* * *

### Memory Hierarchy
Memory hierarchy is the pyramid formed when we stratify different implementations of memory [[storage|storage]] and categorize them according to the level of [[abstraction|general.terms.abstraction]] they exist at.
- ex. implementations are anything from circuit-level processor [[resistors|hardware.circuit-board.resistor]] and [[registers|hardware.cpu.register]] to hard drives and USB sticks.
![](/assets/images/2022-03-10-10-59-56.png)

The main idea of memory hierarchy is that the storage at one level serves as a cache for storage at the next lower level

Because of physical laws, larger storage devices are slower than smaller storage devices.
- ex. a disk drive on a typical system might be 100 times larger than the main memory, but might take 10,000,000x longer to read a word from disk than from memory. Similarly a typical register file (CPU) stores only a few hundred bytes of information, as opposed to millions of bytes in main memory. However, the processor can read data from the register file almost 100x faster than from memory.

### Memory Leak
in languages without garbage collection, memory leaks occur when we fail to free up memory (deallocate) after a [[pointer|memory.pointer]] no-longer serves its purpose (such as when we never reference that variable again until end of execution)

### Memory Cell
The memory cell is the fundamental building block of memory.
The memory cell itself is an electronic circuit that stores a single bit of binary information (ie. 0 or 1)
- This is achieved by sending either a high voltage signal (1) or low signal (0)
