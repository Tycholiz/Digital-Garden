
CAP theorem states that it is impossible for a distributed system to simultaneously provide all three of: consistency, availability, and partition tolerance. Only 2 can be achieved.
- We cannot build a general data store that is continually available, sequentially consistent, and tolerant to any partition failures.
- Therefore, the theorem can really be stated as: "In the presence of a network partition, a distributed system must choose either Consistency or Availability."
    - note: this seems to be at least somewhat of a controversial tone, given that SQL databases occupy CA. This view above would state that CA systems are incoherent, though this opinion might not appear to be reputable, given the prevalence of relational databases.
    - regarding CA systems then, what CAP theorem would argue is that these systems have an inherent weakness, which is that in the case of a network partition, they will be forced to give up either consistency or availability.

### Consistency (C) 
All nodes see the same data at the same time. This means users can read or write from/to any node in the system and will receive the same data. It is equivalent to having a single up-to-date copy of the data.

Depending on the business needs of the application, we may wish to make the trade-off of sacrificing consistency for availability. Say we are implementing a facebook newsfeed. It is acceptable for the application to miss some data points here and there. For instance, if a friend of yours uploads a new post, it's not of critical importance that you get that data right away. That is, if one client is getting its data from a data store that is not up-to-date, then it's not the end of the world (assuming everything can be made up-to-date in a timely manner).

### Availability (A) 
Availability means every request received by a non-failing node in the system must result in a response. Even when severe network failures occur, every request must terminate. In simple terms, availability refers to a system’s ability to remain accessible even if one or more nodes in the system go down.

In an AP (Availability/Partition-tolerant) system, the system is essentially saying “I will get you to a node, but I do not know how good the data you find there will be”; or “I can be available and the data I show will be good, but not complete.”

### Partition tolerance (P) 
a.k.a robustness

Partition tolerance is the ability of a data processing system to continue processing data even if a network partition causes communication errors between subsystems
- A single node failure should not cause the entire system to collapse.
- A partition is a communication break (or a network failure) between any two nodes in the system, i.e., both nodes are up but cannot communicate with each other. 

A partition-tolerant system continues to operate even if there are partitions (ie. communication breakdowns) in the system. Such a system can sustain any network failure that does not result in the failure of the entire network. Data is sufficiently replicated across combinations of nodes and networks to keep the system up through intermittent outages.

![](/assets/images/2021-10-12-10-38-14.png)

## PACELC Theorem
One place where the CAP theorem is silent is what happens when there is no network partition? What choices does a distributed system have when there is no partition?

The PACELC theorem states that in a system that replicates data:
- if there is a partition (‘P’), a distributed system can tradeoff between availability and consistency (i.e., ‘A’ and ‘C’);
- else (‘E’), when the system is running normally in the absence of partitions, the system can tradeoff between latency (‘L’) and consistency (‘C’).
![](/assets/images/2021-10-12-10-43-34.png)

The first part of the theorem (PAC) is the same as the CAP theorem, and the ELC is the extension. The whole thesis is assuming we maintain high availability by replication. So, when there is a failure, CAP theorem prevails. But if not, we still have to consider the tradeoff between consistency and latency of a replicated system.

### Examples
- Dynamo and Cassandra are PA/EL systems: They choose availability over consistency when a partition occurs; otherwise, they choose lower latency.
- BigTable and HBase are PC/EC systems: They will always choose consistency, giving up availability and lower latency.
- MongoDB can be considered PA/EC (default configuration): MongoDB works in a primary/secondaries configuration. In the default configuration, all writes and reads are performed on the primary. As all replication is done asynchronously (from primary to secondaries), when there is a network partition in which primary is lost or becomes isolated on the minority side, there is a chance of losing data that is unreplicated to secondaries, hence there is a loss of consistency during partitions. Therefore it can be concluded that in the case of a network partition, MongoDB chooses availability, but otherwise guarantees consistency. Alternately, when MongoDB is configured to write on majority replicas and read from the primary, it could be categorized as PC/EC.
