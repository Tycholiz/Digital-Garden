
Sharding is a method of splitting and storing a single logical dataset in multiple databases (running on different servers). By distributing the data among multiple machines, a cluster of database systems can store larger dataset and handle additional requests. Sharding is necessary if a dataset is too large to be stored in a single database
- instead of vertically scaling a database with progressively heftier instances, horizontally scale by partitioning data across multiple databases, enabling us to easily spin up additional hosts to accommodate growth
- Sharding is also referred to as horizontal partitioning

With sharding, you would know in which shard the data lies based on the ID.
- illustration: imagine if there were 10 different shards, and the ID of the person was prefixed with a number from 0-9, signifying which shard the data could be found in. This is obviously not how it works, but a database object would have metadata about which shard it could be found in.

Every sharded cluster needs to have some logic in how it will place each piece of data. For instance, you might implement a simple round-robin using modulus:
`photoId % 10`
This will spread the photos evenly around all 10 of the databases, so that where to retrieve them is predictable.

An obvious use-case for sharding is how to store locale information in MMORPGs. Consider that the quest text for a quest in French does not have any benefit to being stored together with the English version of it. This is the nature of your sharding strategy: let's increase lookup speed by removing the amount of rows in our database table. We can create a different table, and call it a shard of the first table. As long as the requester of the data knows which shard the data can be found in, then the result can be substantially faster queries.

Another possibility is that you have a large user table which is accessed heavily. You could create 12 shards for the user data, and give each database server the responsibility over one of the twelve months.

# E Resources
[Sharding Postgres at Notion](https://www.notion.so/blog/sharding-postgres-at-notion)
