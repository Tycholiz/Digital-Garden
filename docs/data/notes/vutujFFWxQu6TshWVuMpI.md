
## What is it?
A distributed system is a system whose components are spread across many machines on a single [[network|network]].
- this network can be anything from a local network to the internet, in cases where we want geographic distribution. 

In distributed computing, a problem is divided into many tasks, each of which is solved by one or more computers, which communicate with each other via [[message passing|general.patterns.messaging]].

Data partition and replication strategies lie at the core of any distributed system.
- [[Replication|db.distributed.replication]] - Keeping a copy of the same data on several different nodes, potentially in different locations. 
  - provides redundancy and improves performance (giving us more nodes to read from).
- [[Partitioning|db.distributed.partitioning]] - Splitting a big database into smaller subsets (partitions) so each can be assigned to different nodes 
  - ex. [[sharding|db.distributed.partitioning.sharding]].

A [[client-server architecture|general.arch.client-server]] might be thought of as the simplest implementation of a distributed system.

A distributed system is not to be confused with [[decentralized architecture|general.arch.decentralized]].
- Distributed means that the processing is shared across multiple nodes, but the decisions may still be centralized and use complete system knowledge.

Distributed systems typically have the following characteristics:
- [[concurrency|general.concurrency]] of components
- lack of a global clock
- failure of components

### From a Single computer to a distributed network
With a single computer, when the hardware is working correctly, the same operation always produces the same result (it is deterministic). If there is a hardware problem, the result is normally a total system failure. Alas, an individual computer with properly functional software is either fully functional or entirely broken (this is deliberate, so as to simplify troubleshooting).

In a distributed system, some computers may be broken in some unpredictable way while others are working fine. This is known as *partial failure*, and they add considerable difficulty because they are a class of failure that is *nondeterministic*.
- sometimes we may not even know if something failed or succeeded, since the time it takes for a message to travel across a network is also nondeterministic. If you send a request and don’t get a response, it’s not possible to distinguish whether (a) the request was lost, (b) the remote node is down, or (c) the response was lost.
  - this can be summed up as *"problems in the network cannot reliably be distinguished from problems at a node"*.
- this nondeterminism and possibility of partial failure is what adds difficulty to distributed systems.

Building a distributed system is about building a reliable system from unreliable components.
- anal: [[protocol.TCP]] is a relible protocol built on the unreliable [[protocol.IP]].

#### Consensus
The difficulty involved in reaching consensus amongst all areas of a distributed system is one of the most important and fundamental problems of architecting a distributed system. 

The consensus problem is normally formalized as follows: one or more nodes may propose values, and the consensus algorithm decides on one of those values. Everyone decides on the same outcome, and once you have decided, you cannot change your mind. 
- In an example of seat-booking of an airplane, when several customers are concurrently trying to buy the last seat, each node handling a customer request may propose the ID of the customer it is serving, and the decision indicates which one of those customers got the seat.

a consensus algorithm must satisfy the following properties 
- Uniform agreement - No two nodes decide differently.
- Integrity - No node decides twice.
- Validity - If a node decides value v, then v was proposed by some node.
- Termination - Every node that does not crash eventually decides some value.

Consensus much be achieved for...
- *leader election*. In a database with single-leader replication, all nodes must agree on which node is the leader.
- *atomic commits*. In a database that supports transactions spanning several nodes/partitions, we have the possibility of a transaction failing on some nodes but succeeding on others. If we want to maintain transaction [[atomicity|db.acid.atomicity]], we must have all nodes in agreement about the outcome of a transaction (either they all abort+rollback or commit).

Zookeeper and [[k8s.node.master.components.etcd]] provide consensus algorithms.

## Why do it?
Key characteristics of a distributed system include Scalability, Reliability, Availability, Efficiency, and Manageability.

## How does it work?
Messages may be passed between nodes of a distributed system by using technologies like HTTP, [[RPCs|deploy.distributed.RPC]], or [[message queues|general.patterns.messaging]]

Historically, *distributed* referred to the fact that there were multiple computers all working in unison to solve some problem. Nowadays, the term is much broader, and can be used to refer to a system of multiple autonomous processes existing on a single machine. In this case, a node of the "network" would be a single process, rather than an entire computer.
- In the case of distributed computing, a node is simply something that passes messages and has its own local memory.

There is no clear distinction between distributed systems and parallel systems, and there is a lot of overlap, thought one distinction is that the nodes of a parallel system all share the same memory, while nodes of a distributed system manage their own:
![Distributed system vs Parallel system](/assets/images/2021-07-16-13-10-56.png)
- Examples of distributed systems: MMOs, the internet itself

## Considerations for distributed systems
According to [[CAP theorem|deploy.distributed.CAP-theorem]], out of consistency, availability and partition tolerance, you can only really achieve 2/3.

### Problems specific to distributed systems
The typical problems in a distributed system are the following:
- maintaining the system state (liveness of nodes)
- communication between nodes

The potential solutions to these problems are as follows:
- centralized state management service (eg [[zookeeper]], [[k8s.node.master.components.etcd]])
  - A centralized state management service such as Apache Zookeeper can be configured as the service discovery to keep track of the state of every node in the system
  - provides a strong consistency guarantee, the primary drawbacks are the state management service becomes a single point of failure and runs into scalability problems for a large distributed system
- peer-to-peer state management service
  - The peer-to-peer state management approach is inclined towards high availability and eventual consistency. The gossip protocol algorithms can be used to implement peer-to-peer state management services with high scalability and improved resilience 1.

# UE Resources
- [Distributed Systems in One Lesson (Kafka Guy)](https://www.youtube.com/watch?v=Y6Ev8GIlbxc)
- [Considerations for distributed systems](https://www.aosabook.org/en/distsys.html)
- [Distributed Transactions Without Atomic Clocks](https://vimeo.com/545130381)
