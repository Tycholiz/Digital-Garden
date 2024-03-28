
## What is it?
A "thread" is a series of instructions that must be run in a row, but your computer may switch which thread is being run at any time. 
- Threads are most commonly a part of [[OS]] or a part of a language runtime.

Threads are the unit of execution on a processor. When a program is run on your computer, that starts up a process. That process can then be made up of one or more threads which execute different tasks.

[[scheduled|os.scheduling]] work is carried out on threads.
- Therefore a thread is the smallest amount of instructions that can be handled by a scheduler.
	- think of it as the atomic unit from the scheduler's perspective.

A [[process|os.process]] is an instance of a program that is being executed on 1 or more threads.
- multiple threads can exist on a process simultaneously.
	- the number of threads depends on the programming language and source code that the process is running on.
- threads within a process share resources like memory, but threads in different processes do not share resources.
- a process can be forked (split) into many child processes

### Analogy: Cooks in a kitchen
Just as a group of cooks can work together in a kitchen to prepare multiple dishes at once, threads can work together in a computer to perform multiple tasks simultaneously. Each cook in the kitchen can focus on a specific task, such as chopping vegetables, cooking meat, or preparing a sauce, and by working together, they can prepare a meal more efficiently and quickly than a single cook could do alone. 
- Similarly, threads can be assigned different tasks to perform simultaneously, such as processing user input, updating a graphical user interface, or running a background task, and by working together, they can improve the performance and responsiveness of a computer program.

However, just as the cooks in a kitchen need to coordinate with each other to avoid getting in each other's way or duplicating efforts, threads in a computer also need to be managed and coordinated to avoid conflicts or inefficiencies. This is typically handled by the [[OS]] or programming language runtime, which schedules and manages the execution of threads and ensures that they have access to the resources they need to perform their tasks.

## Why use them?
**concurrency**: Threads enable [[concurrency|general.concurrency]] which is important in distributed systems. Concurrency allows us to schedule multiple tasks on a single processor. These tasks are run in an interleaved manner and essentially share CPU time between themselves. 
- ex. with I/O concurrency, instead of waiting for an I/O operation to complete before continuing execution (thereby rendering the CPU idle), threads allow us to perform other tasks while we wait.

**Parallelism**: We can perform multiple tasks in parallel on several cores. Unlike with just concurrency where only one task is making progress at a time (depending on which has its share of CPU time at that instant), parallelism allows multiple tasks to make process at the same time since they are executing on different CPU cores.

**Convenience**: Threads provide a convenient way to execute short-lived tasks in the background 
- ex. a master node continuously polling a worker to check if it's alive.

### Multi-process vs Multi-threaded
If a single process in a multi-process application crashes, that process alone dies. The buffer is flushed, and all the other child processes continue happily along.  In a multi-threaded environment, when one thread dies, they all die.
- On some OSs, processes tend to be more expensive than threads (though on Linux, the difference is not that great)

### Thread safety
*Thread safe* means that you can access the resource in multiple threads without worrying about the final state.
- Something thread safe is designed to deal with the possibility the same section of code will be run at the same time by two different threads or the same variable will be used or altered by a different thread.
	- anal: [[general.concurrency]]

Imagine you have a list of names. You have two threads that want to add to the list of names. The *non*-thread safe version can go like this:
```log
Thread 1 reads the list of names [a,b,c]
Thread 2 reads the list of names [a,b,c]
Thread 1 adds d to its list and writes it back out [a,b,c,d]
Thread 2 adds e to its list and writes it back out [a,b,c,e]
```

Because thread 1's read and write operation got interrupted, "d" is not in the final list. If the `addName()` operation was thread safe, this wouldn't have been possible. It would go like this:

```log
Thread 1 adds d to the list [a,b,c,d]
Thread 2 adds e to the list [a,b,c,d,e]
```
