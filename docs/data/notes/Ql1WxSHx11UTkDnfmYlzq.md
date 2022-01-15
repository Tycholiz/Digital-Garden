
- The idea that we have business-logic that we can share with Postgres, and have it implement those guarantees for us. 
	- ex. each employee must have a non-negative integer for their salary
- Includes things like what data types we use, the shape of our schema, what constraints we have (not null, fkey etc.), relations...
	- ex. The schema and the data types. When we define these structures in our database, all data that is entered must comply with the rules set out. For instance, if we have an id column of type int, then the data entering *must* be an int.
	- ex. the fact that MongoDB is schemaless means that we do not have Consistency. Nor does it have transactions (the A and I)
