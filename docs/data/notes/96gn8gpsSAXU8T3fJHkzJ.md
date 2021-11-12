
scheduling is the action of assigning resources to perform tasks
- The resources may be processors, network links or expansion cards. 
- The tasks may be threads, processes or data flows.

### Cooperative multitasking (non-preemptive multitasking)
- In this scheduling paradigm, the OS has it's role as a scheduler reduced. Instead of the scheduler determining which process' turn it is to get run, the processes themselves voluntarily perform this duty. That is, each process knows when its turn is over, and cedes processing power to the next process in line.
- In this scheme, the process scheduler of an operating system is known as a cooperative scheduler, having its role reduced down to starting the processes and letting them return control back to it voluntarily

This paradigm is widely used in memory-constrained embedded systems