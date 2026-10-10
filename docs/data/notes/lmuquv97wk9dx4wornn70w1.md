
Causal consistency is a consistency model whereby all participants of a system (say database [[replications|db.distributed.replication]]) observe causally-related operations in a common order. That is, they all agree on the order of the causally-related operations.
- if 2 operations are causally unrelated (that is, one doesn't cause the other), then the participants don't care about preserving their order. It only makes an effort to agree on the related operations.
  - if all participants *do* have to agree on every single operation, then that is known as *sequential consistency*.

Causal means "the possibility of being causally related". That is, if Process A performs a write operation, followed by Process B performing a write operation, we say that A "potentially causes" or "causally precedes" B. This means every participant in the system must agree on that order.

Causal consistency is a variant of eventual consistency and emerges as a middle ground between eventual consistency and strong consistency

The write operations that are causally unrelated or occur in parallel in real-time are known as concurrent events. The causal consistency pattern does not guarantee ordering for concurrent events

The opposite of causal is concurrent (or, causally independent).

Apache Cassandra provides lightweight transactions with causal consistency

### Use-cases
In Reddit, replies to the same comment thread must be causally ordered. However, unrelated comment threads can be shown in any order. 

The causal consistency pattern is also used in real-time chat services such as Slack.

A comment thread on social media platforms such as Reddit. 
- Replies to the same comment thread on Reddit must be causally ordered. However, unrelated comment threads can be shown in any order