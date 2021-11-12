
A distributed system is a system whose components are spread across many machines on a single network. Therefore, when we think of a distributed system we think of it as a network— rather than a single computer.

Data partition and replication strategies lie at the core of any distributed system.

Distributed systems typically have the following characteristics:
- concurrency of components
- lack of a global clock
- failure of components

Key characteristics of a distributed system include Scalability, Reliability, Availability, Efficiency, and Manageability. 

Messages may be passed between nodes of a distributed system by using technologies like HTTP, [[RPCs|deploy.distributed.RPC]], or message queues

Historically, *distributed* referred to the fact that there were multiple computers all working in unison to solve some problem. Nowadays (as of 2021), the term is much broader, and can be used to refer to a system of multiple autonomous processes existing on a single machine. In this case, a node of the "network" would be a single process, rather than an entire computer.
- In the case of distributed computing, a node is simply something that passes messages and has its own local memory.

There is no clear distinction between distributed systems and parallel systems, and there is a lot of overlap, thought one distinction is that the nodes of a parallel system all share the same memory, while nodes of a distributed system manage their own:
![Distributed system vs Parallel system](/assets/images/2021-07-16-13-10-56.png)
Examples of distributed systems: MMOs

## Considerations for distributed systems
According to [[CAP theorem|deploy.distributed.CAP-theorem]], out of consistency, availability and partition tolerance, you can only really achieve 2/3.


* * *

### Distributed Databases
Imagine having billions of users, along with trillions of other pieces of data associated with these users. Of course, a single machine is not enough to handl

# UE Resources
[Good Speaker (Kafka Guy)](https://www.youtube.com/watch?v=Y6Ev8GIlbxc)
[Considerations for distributed systems](https://www.aosabook.org/en/distsys.html)
[Distributed Transactions Without Atomic Clocks](https://vimeo.com/545130381)
