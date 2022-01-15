
# DynamoDB
Dynamo is a fully-managed schemaless NoSQL database.
- The DB engine can manage structured or semi-structured data, including JSON documents.
- supports key–value and document data structures

Uses tables

Data is stored as a partitioned B-Tree.

Unlike Redis, in that it is immediately consistent and highly-durable, centered around that single data structure.
- If you put something into DynamoDB, you’ll be able to read it back immediately and, for all practical purposes, you can assume that what you have put will never get lost.

provides seamless integration with services such as Redshift (large scale data analysis), Cognito (identity pools), Elastic Map Reduce (EMR), Data Pipeline, Kinesis, and S3. Also, has tight integration with AWS lambda via Streams and aligns with the server-less philosophy; automatic scaling according to your application load, pay-per-what-you-use pricing, easy to get started with, and no servers to manage.

## Terms
### AttributeName
column name of a table

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

## DynamoDB in practice

The general rule of thumb is to choose Dynamo for low throughput apps as writes are expensive and consistent reads are twice the cost of eventually consistent reads

When to use DynamoDB?
- In case you are looking for a database that can handle simple key-value queries but those queries are very large in number
- In case you are working with OLTP workload like online ticket booking or banking where the data needs to be highly consistent

When not to use DynamoDB?
- In cases where you have to do computations on the data.
	- Relational databases run their queries close to the data, so if you’re trying to calculate the sum total value of orders per customer, then that rollup gets done while reading the data, and only the final summary (one row per customer) gets sent over the network. However, if you were to do this with DynamoDB, you’d have to get all the customer orders (one row per order), which involves a lot more data over the network, and then you have to do the rollup in your application, which is far away from the data.
- If worried about high vendor lock-in.

Pricing
\$256/TB/month

By default, you should start with DynamoDB’s on-demand pricing and only consider the provisioned capacity as cost optimization. On-demand costs \$1.25 per million writes, and \$0.25 per million reads.
- Then, if your usage grows significantly, you will almost always want to consider moving to provisioned capacity (significant cost savings).
- if you believe that on-demand pricing is too expensive, then DynamoDB will very likely be too expensive, even with provisioned capacity. In that case, you might want to consider a relational database.
