
Sharding is a special type of [[horizontal partitioning|db.distributed.partitioning#horizontal-partitioning-aka-range-based-partitioning]] where partitions spans across multiple [[database replicas|db.distributed.replication]].

Sharding is a method of splitting and storing a single logical dataset in multiple databases (running on different servers). By distributing the data among multiple machines, a cluster of database systems can store larger dataset and handle additional requests. Sharding is necessary if a dataset is too large to be stored in a single database
- instead of vertically scaling a database with progressively heftier instances, horizontally scale by partitioning data across multiple databases, enabling us to easily spin up additional hosts to accommodate growth

Each shard is responsible for storing and managing one or more chunks of data.
- a chunk is a contiguous range or subset of data that is stored within a specific shard.
- purpose is to allow for more efficient data distribution and management across the shards in a sharded database.
- As the data changes or the sharding configuration is modified (e.g., adding new shards), chunk migration may occur to rebalance data distribution.

With sharding, you would know in which shard the data lies based on the ID.
- illustration: imagine if there were 10 different shards, and the ID of the person was prefixed with a number from 0-9, signifying which shard the data could be found in. This is obviously not how it works, but a database object would have metadata about which shard it could be found in.

Every sharded cluster needs to have some logic in how it will place each piece of data. For instance, you might implement a simple round-robin using modulus:
- `photoId % 10` - This will spread the photos evenly around all 10 of the databases, so that where to retrieve them is predictable.

### Shard Key
The shard key is a column (in SQL) or field (in NoSQL) value that determines which shard a given row of data will exist in.

What makes a good candidate for shard key?
- The resulting shards should hold about the same amount of data so we don't get hotspots
- Choose a shard key with high cardinality. A shard key with low cardinality reduces the effectiveness of horizontal scaling in the cluster.
    - imagine the column value being an enum. How many different values could it be?
    - ex. imagine our data table had a field `continent`, and our strategy was to shard based on which continent the user lived on. This would mean our shard key has a cardinality of `7`, meaning there can be no more than `7` chunks within the sharded cluster
- Consider the most common types of queries performed in your application and how the shard key impacts query performance. The shard key should ideally align with the most frequent query patterns to minimize cross-shard queries.
    - ex. if your application frequently performs range queries on a specific attribute, using that attribute as the shard key can improve performance.
- Analyze the write and update patterns in your application. Consider how the shard key affects write and update operations.
    - ex. if you expect a high volume of writes for a specific set of data, distributing that data across multiple shards based on the shard key can improve write throughput.
- A good shard key should be chosen based on the application's query patterns to isolate queries to specific shards. Queries that involve the shard key in the filter can be efficiently executed on a single shard, minimizing the need to search across all shards.


### Use-cases
An obvious use-case for sharding is how to store locale information in MMORPGs. Consider that the quest text for a quest in French does not have any benefit to being stored together with the English version of it. 
- This is the nature of your sharding strategy: "let's increase lookup speed by removing the amount of rows in our database table. We can create a different table, and call it a shard of the first table. As long as the requester of the data knows which shard the data can be found in, then the result can be substantially faster queries."

Another possibility is that you have a large user table which is accessed heavily. You could create 12 shards for the user data, and give each database server the responsibility over one of the twelve months.

## E Resources
- [Sharding Postgres at Notion](https://www.notion.so/blog/sharding-postgres-at-notion)
