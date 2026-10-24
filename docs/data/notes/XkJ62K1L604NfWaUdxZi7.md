
The CPU is the engine that interprets (or executes) instructions that are stored in main memory.
- at its core, there is a word-sized storage device (or register) called the program counter (PC).
	- At any given point in time, the PC points at (contains the address of) some machine-language instruction in main memory.
- From the time the system is powered on until it is powered off, the processor blindly and repeatedly performs the same basic task over and over again:
	1. reads the instruction from memory pointed at by the PC
	2. interprets the bits in teh instruction
	3. performs some simple operation, as per the instruction
	4. updates the PC to point to the next instruction (which may or may not be in contiguous memory) 
- Different CPU architectures understand different instruction types, which is why we can't take a program from one architecture (ex. x86) and run it on another (ex. ARM)

Typically a CPU communicates with devices via a [[bus|hardware.bus]].

- A socket is the physical socket where the physical CPU capsules are placed
- Cores are the number of CPU-cores per CPU capsule
- some CPUs can run more than one parallel thread per CPU-core
- If you multiply the number of socket, cores and threads, then you get the number of "CPUs": 24.
	- These aren't real CPUs, but the number of possible parallel threads of execution your system can do.

The CPU reads instructions in, does the instruction, and then reads the next instruction. These instructions need addresses (RAM address) to know where to go to get data, or put data. But it also needs to know what to do with that data. These two parts compose a basic "instruction", usually divided into op-codes(operation code) and addresses.
- So a 32 bit CPU has 32 bits for an instruction, while a 64 bit CPU has 64 bits to hold an instruction. This means we can have many more opcodes, and also have many more places in memory we can address.
- ex. Let's say numbers 1 through 20 are addresses in RAM. If we have, for example, a 3 bit processor, an instruction would look like so: S-1-2 (something like subtract 1 from 2, assuming S is subtract op-code). Now what if we want to subtract something from memory address 17? We can't, we don't have enough room, since we only have 3 bits(1 for opcode, one for first address, one for second address). There are ways to work around this but for simplicity, it's impossible.

### Hardware Acceleration
