
Dynamo is a fully-managed, highly available schemaless NoSQL database.
- The DB engine can manage structured or semi-structured data, including JSON documents.
- supports key–value and document data structures

Uses tables, though this concept is loosely related to how tables are used in SQL.
- essentially, we would include all domain primitives of a single domain in a Dynamo table, and use [[GSIs|aws.svc.dynamo#global-secondary-index-gsi,1:#*]] as our means of "JOINing" the domain primitives together.
    - each GSI should represent an access pattern (ie. how that data is accessed, such as "get all people by gender", "get all last names by country")
- One of the biggest mistakes people make with dynamo is thinking that it's just a relational database with no relations. It's not.

To get the full benefits of Dynamo, and it requires you often to design your data layer very well up-front. Dynamo is not recommended for a system that hasn't mostly stabilized in design.

### When to use Dynamo
Dynamo is really good for high read:write ratio (at least 4:1), meaning Dynamo is a good candidate for high-read applications.
- By default each DynamoDB table is allocated 40,000 read units and 40,000 write units of capacity per second. 

Dynamo may support large, complex schemas but it gets more difficult to maintain and understand. Dynamo is a better candidate for applications with simpler schemas.

Dynamo offers effortless cross-region replication. Therefore, it is a good candidate for apps that distributed geographically.

## Data is stored as partitions
Data is stored as a partitioned [[general.lang.data-structs.tree.B]].
- Items are distributed across 10-GB storage units, called partitions (physical storage internal to DynamoDB)
- Each table has one or more partitions
- DynamoDB uses the partition key’s value as an input to an internal hash function. The output from the hash function determines the partition in which the item is stored. Each item’s location is determined by the hash value of its partition key.

Unlike [[redis]], in that it is immediately consistent and highly-durable, centered around that single data structure.
- If you put something into DynamoDB, you’ll be able to read it back immediately and, for all practical purposes, you can assume that what you have put will never get lost.

## Terms
### Items
Analogous with row of a SQL table

### Attribute
Analogous with column name of a SQL table

### Marshalling
Before we can Create/Update a record in DynamoDB, a plain JS Object needs to be converted into a DynamoDB Record.
- Marshalling refers to our ability to convert a Javascript object into a DynamoDB Record.

#### Example
```js
AWS.DynamoDB.Converter.unmarshall({
    "updated_at":{"N":"146548182"},
    "uuid":{"S":"foo"},
    "status":{"S":"new"}
})

// { updated_at: 146548182, uuid: 'foo', status: 'new' }
```

* * *

Dynamo provides seamless integration with services such as Redshift (large scale data analysis), Cognito (identity pools), Elastic Map Reduce (EMR), Data Pipeline, Kinesis, and S3. Also, has tight integration with AWS lambda via Streams and aligns with the server-less philosophy; automatic scaling according to your application load, pay-per-what-you-use pricing, easy to get started with, and no servers to manage.

* * *

## Expression
[[aws.svc.dynamo.expression]]

* * *

## DynamoDB in practice

The general rule of thumb is to choose Dynamo for low throughput apps as writes are expensive and consistent reads are twice the cost of eventually consistent reads

When to use DynamoDB?
- In case you are looking for a database that can handle simple key-value queries but those queries are very large in number
- In case you are working with OLTP workload like online ticket booking or banking where the data needs to be highly consistent

When not use DynamoDB?
- In cases where you have to do computations on the data.
	- Relational databases run their queries close to the data, so if you’re trying to calculate the sum total value of orders per customer, then that rollup gets done while reading the data, and only the final summary (one row per customer) gets sent over the network. However, if you were to do this with DynamoDB, you’d have to get all the customer orders (one row per order), which involves a lot more data over the network, and then you have to do the rollup in your application, which is far away from the data.
- If worried about high vendor lock-in.

Pricing
\$256/TB/month

By default, you should start with DynamoDB’s on-demand pricing and only consider the provisioned capacity as cost optimization. On-demand costs \$1.25 per million writes, and \$0.25 per million reads.
- Then, if your usage grows significantly, you will almost always want to consider moving to provisioned capacity (significant cost savings).
- if you believe that on-demand pricing is too expensive, then DynamoDB will very likely be too expensive, even with provisioned capacity. In that case, you might want to consider a relational database.

## UE Resources
- [Determining your data model in Dynamo](https://docs.aws.amazon.com/prescriptive-guidance/latest/dynamodb-data-modeling/steps.html)
- [Dynamo whitepaper](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)