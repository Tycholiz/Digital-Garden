
# Isolated
- the opposite side of atomicity.
- while we are doing our queries, are we allowed to see what is happening concurrently in the rest of the system?
	- ex. what if we want to make a backup with `pg_dump` that needs to run for several hours? That backup needs to be a consistent snapshot of the production database. If during the backup someone is doing inserts, we don't want these to be in the backup, since we want a snapshot that doesn't move.		- to do this, postgres uses an isolation mode that prevents this from happening.	
