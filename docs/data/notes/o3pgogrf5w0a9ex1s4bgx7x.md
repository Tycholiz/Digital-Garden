
Scheduling is the action of assigning resources to perform tasks. 
- The resources may be processors, network links or expansion cards. 
- The tasks may be threads, processes or data flows.

Schedulers are designed so as to keep all computer resources busy
- therefore, they analaglous to [[load balancers|deploy.distributed.load-balancer]]

Schedulers also manage how each user shares system resources effectively.

Scheduling is fundamental to computation itself, and an intrinsic part of the execution model of a computer system

* * *

### Kernel thread
A kernel thread is a "lightweight" unit of kernel scheduling

At least one kernel thread exists within each process. 
- If multiple kernel threads exist within a process, then they share the same memory and file resources. 

Kernel threads are preemptively multitasked if the operating system's process scheduler is preemptive. Kernel threads do not own resources except for a stack, a copy of the registers including the program counter, and thread-local storage (if any), and are thus relatively cheap to create and destroy.
- Thread switching is also relatively cheap

* * *

### Cooperative multitasking (non-preemptive multitasking)
- In this scheduling paradigm, the OS has its role as a scheduler reduced. Instead of the scheduler determining which process' turn it is to get run, the processes themselves voluntarily perform this duty. That is, each process knows when its turn is over, and cedes processing power to the next process in line.
- In this scheme, the process scheduler of an operating system is known as a cooperative scheduler, having its role reduced down to starting the processes and letting them return control back to it voluntarily

This paradigm is widely used in memory-constrained embedded systems
- the concept of scheduling makes it possible to have computer multitasking with a single [[CPU|hardware.cpu]].