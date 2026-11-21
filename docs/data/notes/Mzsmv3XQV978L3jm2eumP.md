
Concurrency control is about the database ensuring that correct results for concurrent operations are generated, while getting those results as quickly as possible

Without concurrency control, if someone is reading from a database at the same time as someone else is writing to it, it is possible that the reader will see a half-written or inconsistent piece of data.
- ex. when making a wire transfer between two bank accounts if a reader reads the balance at the bank when the money has been withdrawn from the original account and before it was deposited in the destination account, it would seem that money has disappeared from the bank.

[[Isolation|db.acid.isolation]] is the property that provides guarantees in the concurrent access to data
- implemented by means of a concurrency control protocol. 

The simplest way to achieve concurrency is to make all readers wait until the writer is done (*read-write lock*).