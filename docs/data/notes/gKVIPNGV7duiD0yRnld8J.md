
To implement master-slave replication, there are several things that must happen
1. Ordering
	- Imagine if two transactions were to be applied sequentially to a database:
		— the first writes a new entry and the second deletes this entry, which ultimately results in no data persisting in the database
		- if the ordering is not guaranteed, the delete transaction could be processed first (causing no effect) and then the write transaction applied, which results in data incorrectly persisting in the database.
