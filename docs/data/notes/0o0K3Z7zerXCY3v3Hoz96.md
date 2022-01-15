
# Normalization
- Denormalization is the process whereby you put data often used together in a single table to increase performance, at the sake of some database purity. Many find it to be an acceptable trade, even going so far as to design the schema intentionally denormalized to skip the intermediary step of having to join tables.
- degree of normalization is defined as ~6 levels called Normal Forms (NF)
	- getting to level 6 (6NF) is not necessarily better than just getting to level 3 (3NF). "more normalization" does come at a cost.
	- ex. 6NF requires tables to have "1 PK, and at most 1 attribute"
- purpose is to reduce data redundancy and improve data integrity
- Normalization entails organizing the columns (attributes) and tables (relations) of a database to ensure that their dependencies are properly enforced by database integrity constraints. It is accomplished by applying some formal rules either by a process of synthesis (creating a new database design) or decomposition (improving an existing database design).
	- more normalization means more tables
	- more "normal" schemas have less attributes when designing a schema, the goal is to find the right balance of normalization: too much: you have a proliferation of tables; too little: you have coupled, repeated, and difficult to query data
- A hypothetical example of a failure to properly normalize SQL tables would be a hospital database having a table of patients which included a column for the telephone number of their doctor. The phone number is dependent on the doctor, rather than the patient, thus would be better stored in a table of doctors. The negative outcome of such a design is that a doctor's number will be duplicated in the database if they have multiple patients, thus increasing both the chance of input error and the cost and risk of updating that number should it change (compared to a third normal form-compliant data model that only stores a doctor's number once on a doctor table)

## How much normalization?
- A good rule of thumb for whether or not to normalize is to think about if the attribute might be updated in the row's lifetime. If the attribute probably won't be, then it's fine to denormalize.
	- ex. imagine Medium.com, which has the concept of organizations (eg. FreeCodeCamp) that can post blogs. Since we can be reasonably assured that a post will not change the organization it is attached to, it's probably ok to denormalize `organization_id` into `posts` (ie. organization_id becomes a column of the posts table)
- Normalization can be embraced more when a table has a lower row count.
- Denormalization can be embraced when a table has many rows and/or has expensively calculated properties that are accessed often.
	- we should make use of triggers to automatically update attributes in other tables where data has been duplicated as a result of denormalization. In this case, we can prefix the columns with `gen_` (generated), to be explicit about the fact that this column should not be updated by hand, but is a column that is only updated as a result of the trigger.
- If we have a write heavy table that is not read often, the overhead of adding denormalized data at write time might decrease performance more than it improves read performance.
