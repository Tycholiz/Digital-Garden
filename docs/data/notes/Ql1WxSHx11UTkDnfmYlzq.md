
Consistency refers to an application-specific notion of the database being in a "good state".

Consistency is the idea that we have business-logic that we can share with the database, and have it implement those guarantees for us. 
- ex. each employee must have a non-negative integer for their salary
- ex. in an accounting system, credits and debits must be balanced.
- Includes things like what data types we use, the shape of our schema, what constraints we have (not null, fkey etc.), relations...
	- ex. The schema and the data types. When we define these structures in our database, all data that is entered must comply with the rules set out. For instance, if we have an id column of type int, then the data entering *must* be an int.
	- ex. the fact that MongoDB is schemaless means that we do not have Consistency. Nor does it have transactions (the A and I)

The application may rely on the database’s [[atomicity|db.acid.atomicity]] and [[isolation|db.acid.isolation]] in order to achieve consistency, but it’s not up to the database alone.

A [[transaction|db.strategies.transaction]] either creates a new and valid state of data, or, if any failure occurs, returns all data to its state before the transaction was started.

## UE Resources
- [Consistency](https://systemdesign.one/consistency-patterns/)


Don Ross 