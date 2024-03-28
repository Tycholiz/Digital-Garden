
A *distributed lock manager* (DLM) runs in every machine in a cluster

## Purpose
- [source](https://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html)

The purpose of a lock is to ensure that 2+ nodes in a distributed system are not doing the same work. That work might be to write some data to a shared storage system, to perform some computation, to call some external API etc.

At a high level, there are 2 reasons you might want a lock: efficiency, or ensure correctness.
- the efficiency problem is less worrysome, and usually results in nothing more than a nuisance. For instance, a user might get 2 password reset emails, or we might make 2 database reads for the same data.
- the correctness problem is the one to be concerned about. If 2 nodes concurrently work on the same piece of data, we may very well result in a corrupted file, data loss, or permanent inconsistency.
