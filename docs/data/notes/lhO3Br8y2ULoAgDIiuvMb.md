
A subshell starts out as an almost identical copy of the original shell process. Under the hood, the shell calls the fork system call1, which creates a new process whose code and memory are copies2. When the subshell is created, there are very few differences between it and its parent. In particular, they have the same variables

A subshell is different from executing a script. 
- A script is a separate program. This separate program coincidentally might also be a script which is executed by the same interpreter as the parent, but this doesn't mean the separate program has any special visibility on internal data of the parent. This is a mere coincidence.