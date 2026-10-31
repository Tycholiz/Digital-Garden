
The client-side library for local persistence is called Realm Database, while the associated Firebase-like backend service is called Realm Object Server.

## Partitioning
Mongo needs a way to know which data it should give the user access to. Of course, any one user should not receive all of the data in the entire application, only what is owned by them. When using RealmDB, each partition is composed of Realms.
- A realm is a collection of Realm objects that all share a *partition key*. Each client subscribes to a different set of realms, which passively synchronize changes as network availability allows.realm
- each partition value (ex. one userId) maps to a different realm. Therefore, Documents that share the same partition value belong to the same realm
- Most common realms would be defined by userId or teamId, meaning all the data with the same userId will be partitioned and synced with the RealmDB on the user's phone.
- You can combine these partitioning strategies to create applications that make some data publicly available, like announcements or an organizational chart, but restrict other data to privileged users.
	- To combine strategies, you can use a generic partition key field name like `_partitionKey` with a partition value structured like a query string. For example: `_partitionKey: "user_id=abcdefg"`
		- Structuring your partition values like a query string lets you take advantage of multiple partitioning strategies at once, providing the power of user, team, and public realms all at once

Since all documents that share the same partition value also share the same permissions for each user, you should select a key whose unique values correspond with permissions within your application. Consider which documents each user needs to read, and which documents each user needs to write. What separates one user’s data from another? The concepts of ownership (which users can change which data?) and access (which users can see which data?) decide how you should partition your data.
