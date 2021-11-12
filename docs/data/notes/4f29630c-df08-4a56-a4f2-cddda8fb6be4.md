
# Tablespace
- Tablespaces are where PostgreSQL stores the database objects. Therefore, it is an abstraction between the physical and logical layers (ie. what the data looks like on disk, and what the data looks like in sql-format.)
- Tablespaces allow you to move your data to different physical locations across drivers easily by using simple commands.
	- ex. 2 different objects in one schema might have 2 different underlying tablespaces.
- By default, PostgreSQL provides you with two tablespaces:
	1. The `pg_default` is for storing user data.
	2. The `pg_global` is for storing system data.
- A common use of tablespaces is for performance
	- ex. an index that is used often can be tablespaced on a faster SSD. 
	- ex. a table with cold data that is rarely accessed can be tablespaced on a slower magnetic HDD.
- At the file layer (of data storage), a single file can only correspond to a single tablespace.
- Using tablespaces, we can provision different locations for our data, based on the idea of classifying data as hot, warm or cold (determined by frequency that data will be needed)
	- This would involve us having a different tablespace in each storage group (ie. a different path to the physical localtion). This means that moving data from hot to warm involves changing the tablespace that data is associated with.

https://techchannel.com/SMB/9/2012/storage-groups-hot-warm-cold#:~:text=The%20classifications%20are%20often%20referred,stored%20on%20even%20slower%20storage.
