
CRDTs are general-purpose data structures, like [[hash maps|general.lang.data-structs.hash-table]] and [[arrays|general.lang.types.array]], but the special thing about them is that they are multi-user from the ground up.

CRDTs are built with the complexities arising from multiple users on multiple devices in mind. 
- They are also fundamentally local and private.

The only type of change that a CRDT cannot automatically resolve is when multiple users concurrently update the same property of the same object; 
- in this case, the CRDT keeps track of the conflicting values and leaves the responsibility of resolution up to the application code.
- Thus, CRDTs have some similarity to version control systems like Git, except that they operate on richer data types than text files.

Every application needs some data structures to store its document state. For example, if your application is a text editor, the core data structure is the array of characters that make up the document. If your application is a spreadsheet, the data structure is a matrix of cells containing text, numbers, or formulas referencing other cells. If it is a vector graphics application, the data structure is a tree of graphical objects such as text objects, rectangles, lines, and other shapes.
- If you are building a single-user application, you would maintain those data structures in memory using model objects, hash maps, lists, records/structs and the like. If you are building a collaborative multi-user application, you can swap out those data structures for CRDTs.

[[Concurrent|general.concurrency]] updates to multiple [[replicas|db.distributed.replication]] of the same piece of data often results in conflicts.
- as a result, the strategy is often to prevent concurrent updates to replicated data.
- another approach is optimistic replication (a.k.a lazy replication), where replicas are allowed to diverge.

The pessimistic approach guarantees that all replicas are identical to each other. The optimistic approach is of eventual consistency, meaning that we only expect replicas to converge on an identical state when the system has been quiesced for a period of time (ie. paused until all replicas can catch up).
- as a result, there is no need for all replicas to be synchronized before making udpates.
    - this helps concurrency and parallelism.

The downside to this approach is that each replica may require explicit reconciliation later on, which may be difficult.

The idea of this approach is that we will re-establish consistency between replicas eventually (ie. achieve eventual consistency) by merging different replicas.

Optimistic replication does not work in many cases, but it works with CRDTs.
- in fact, it is mathematically always possible to merge or resolve concurrent updates on different replicas of the data structure without conflicts 

### Example: boolean flag
Imagine we had a flag in the database that represented whether or not someone has ever done something. For example, whether someone has ever drank alcohol before. With this particular business logic, once `has_drank_booze` goes to `true`, it can never return to `false`. Therefore, we know that if one replica has a `true` value, then that is the source of truth.
- here the resolution method is "true wins"

## Resources
- https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type
- https://www.inkandswitch.com/local-first/#crdts-as-a-foundational-technology
- https://www.youtube.com/watch?v=iEFcmfmdh2w&t=607s