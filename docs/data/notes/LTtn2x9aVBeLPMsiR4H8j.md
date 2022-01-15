
# Threads
**Scheduling** is the action of assigning work to different resources in a system. 
- this work is carried out on threads.
Therefore a thread is the smallest amount of instructions that can be handled by a scheduler.
- think of it as the atomic unit from the scheuler's perspective.

A **process** is an instance of a program that is being executed on 1 or more threads.
- multiple threads can exist on process simultaneously.
	- the number of threads depends on the programming language and source code that the process is running on.
- threads within a process share resources like memory, but threads in different processes to not share resources.
- a process can be forked (split) into many child processes

### Multi-process vs Multi-threaded
If a single process in a multi-process application crashes, that process alone dies.  The buffer is flushed, and all the other child processes continue happily along.  In a multi-threaded environment, when one thread dies, they all die.

- On some OSs, processes tend to be more expensive than threads (though on Linux, the difference is not that great)
