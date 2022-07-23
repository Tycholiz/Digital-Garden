
Causal consistency is a consistency model whereby all participants of a system (say database [[replications|db.distributed.replication]]) observe causally-related operations in a common order. That is, they all agree on the order of the causally-related operations.
- if 2 operations are causally unrelated (that is, one doesn't cause the other), then the participants don't care about preserving their order. It only makes an effort to agree on the related operations.
  - if all participants *do* have to agree on every single operation, then that is known as *sequential consistency*.

Causal means "the possibility of being causally related". That is, if Process A performs a write operation, followed by Process B performing a write operation, we say that A "potentially causes" or "causally precedes" B. This means every participant in the system must agree on that order.

The opposite of causal is concurrent (or, causally independent).
