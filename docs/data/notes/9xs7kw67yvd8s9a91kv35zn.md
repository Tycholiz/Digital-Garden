
When it comes to high-availability (HA) clusters, the two main replication topologies you’ll encounter in data storage are:
1. Active/active clusters
2. Active/passive clusters

|                          | Active/Active                                                                                                                                                           | Active/Passive                                                                                                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Benefits**             | - Better resource utilization since both nodes can handle traffic.<br>- No failover needed.<br>- Higher availability, with no single point of failure<br/>- Better scalability since load is spread across multiple nodes.<br/>- Easy to scale out by adding more database shards | - Only one node is active at a time, so there is no contention for resources.<br/>- Simplified data consistency as there is only one active node handling write operations.<br>- Simpler configuration and management.<br>- Easier to test failover since it only happens when the active node fails. |
| **Drawbacks**            | - Can be more complex to configure and manage.<br/>- Scaling limitations on a single host.<br/>- Inflexible data model<br/>- hot spots and underutilization of shards due to the difficulty in re-sharding<br>- Synchronization and consistency issues need to be carefully addressed.<br>- Increased latency due to the need to synchronize data between nodes. | - Higher resource utilization since only one node is active.<br>- Failover can take longer since the passive node needs to start up and take over.<br>- Potential for data loss if the active node fails before changes have been replicated to the passive node.|


## Active/Active
In Active/active, client machines connect to a [[load balancer|deploy.distributed.load-balancer]] that distributes their workloads across multiple active servers.
- with active/active, the database can be written to from any region. Therefore if there is a degradation in one of the regions, there won't be any perceived downtime for end users.

### When to use
- Suitable for applications that require high availability and demand continuous operation even during planned or unplanned maintenance or node failures.
- Use active-active when you need to distribute the workload across multiple nodes to handle high traffic and achieve better performance, responsiveness, and throughput.
- Beneficial for mission-critical applications, real-time systems, or cloud services that require seamless scalability and minimal downtime during maintenance.

## Active/Passive
In Active/passive, client machines connect to the main server, which handles the full workload, while a backup server remains on standby, only activating in the event of a failure.
- with active/passive, we’d have some short period of time where nobody could login in (even if they could still keep querying) while we stand up a new primary

### When to use
- Suitable for applications where high availability is critical, but the primary concern is minimizing downtime in the event of a failure or maintenance.

## Analogy: Building Power Generators
An active/active cluster is like having two separate generators that are both running and providing power to the building simultaneously. If one generator were to fail, the other would continue to provide power without interruption. This is similar to a building with redundant power sources that are both actively providing power, and if one source were to fail, the other would seamlessly take over.

On the other hand, an active/passive cluster is like having a primary generator that is providing power to the building, with a secondary backup generator that is on standby in case the primary one fails. In this setup, the secondary generator is not actively providing power to the building until it is needed, at which point it would be switched on and take over from the primary generator. This is similar to a building with a backup generator that is not running until the primary power source fails, at which point the backup generator would start up and provide power.