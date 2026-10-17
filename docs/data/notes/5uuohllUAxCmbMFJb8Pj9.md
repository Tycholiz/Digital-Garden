
A DynamoDB Stream is a time-ordered sequence of modifications made to items in a single DynamoDB table.
- each stream record provides information about the item-level modification that was made.
- When you enable a stream on a table, DynamoDB captures information about every modification to data items in the table.
	- disabled by default
- DynamoDB Streams are near realtime.

A Dynamo Stream is basically Dynamo's way of exposing its table-level [[change logs|general.lang.data-structs.log]].

Data is durably stored for up to 24 hours.

DynamoDB streams are a bit like a direct messaging queue integration (Kinesis/Kafka) with a table that contains all the events that are happening in the table.
- This differs from usual streaming where data can be a complete business object. In a DynamoDB stream, users are limited to the table that triggered the stream events.

DynamoDB streams can be processed with [[AWS Lambdas|aws.svc.lambda]].
- To achieve best separation of concerns, use one Lambda function per DynamoDB Stream. It will help you keep IAM permissions minimal and code as simple as possible

### Endpoints
DynamoDB and DynamoDB Streams are accessed via different endpoints.
- the DynamoDB endpoint lets us work with database tables and indexes
- the DynamoDB Streams endpoint lets us work with records

The naming convention for DynamoDB Streams endpoints is `streams.dynamodb.<region>.amazonaws.com`.
- ex. if you use the endpoint `dynamodb.us-west-2.amazonaws.com` to access DynamoDB, you would use the endpoint `streams.dynamodb.us-west-2.amazonaws.com` to access DynamoDB Streams.

## Problem sets solved by DynamoDB Streams
DynamoDB Streams are great if you want to decouple your application core business logic from effects that should happen afterward.
- Your base code can be minimal while you can still "plug-in" more Lambda functions reacting to changes as your software evolves.

- How do you set up a relationship across multiple tables in which, based on the value of an item from one table, you update the item in a second table?
- How do you trigger an event based on a particular item change?
- How do you audit or archive data?
- How do you replicate data across multiple tables (similar to that of [[materialized views|sql.view.materialized]]/streams/replication in relational data stores)?

DynamoDB Streams works particularly well with AWS Lambda due to its event-driven nature.

## Components of a DynamoDB Stream

The following diagram shows the relationship between a stream, shards in the stream, and stream records in the shard.

![](/assets/images/2021-12-13-14-37-18.png)

### Shard
Stream records from a single stream are organized into uniquely identifiable groups called *shards*.
- Each shard acts as a container for multiple stream records, and contains information required for accessing and iterating through these records.

Shards are ephemeral: They are created and deleted automatically, as needed. Any shard can also split into multiple new shards; this also occurs automatically.

Shards are distributed by nature.

We know ahead of time how many shards there will be. Even if new records are added to the stream after the fact, this number of shards doesn't change.

#### ShardIterator
A shard [[iterator|general.patterns.behavioural.iterator]] provides information about how to retrieve the stream records from within a shard. Use the shard iterator in a subsequent `GetRecords` request to read the stream records from the shard.

Each ShardIterator has a ShardIteratorType which determines how the shard iterator is used to start reading stream records from the shard.
- [(docs)](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_streams_GetShardIterator.html)

a ShardIterator specifies a location within a shard, but that location could be either the oldest/newest/etc so NextShardIterator allows you to determine where the next set of records are to then iterate through

### (Stream) Record
A record is a description of a unique event within a stream.

A record contains information about a data modification to a single item in a DynamoDB table.
- You can configure the stream so that the stream records capture additional information, such as the "before" and "after" images of modified items.

#### Sequence Number
Each record has a sequence number that locates it in chronological order within a given shard.
- it is unique per partition-key within its shard.
	- in the same partition-key, sequence numbers generally increase over time— The longer the time period between write requests, the larger the sequence numbers become.
		- however, there is no guarantee the sequence number will increase by 1.

The sequence number reflects the order in which the record was published to the stream.
- Therefore, the sequence number will stay sequential so long as it is in the same partition.

## How to access individual stream records
To access a stream and process the stream records within, you must do the following:
1. Determine the unique ARN of the stream that you want to access.
2. Determine which shards in the stream contain the stream records that you are interested in.
3. Access the shards and retrieve the stream records that you want.

## Streams API
The DynamoDB Streams API provides the following actions for use by application programs:
- `ListStreams` — Returns a list of stream descriptors for the current account and endpoint. You can optionally request just the stream descriptors for a particular table name.
- `DescribeStream` — Returns detailed information about a given stream. The output includes a list of shards associated with the stream, including the shard IDs.
- `GetShardIterator` — Returns a shard iterator, which describes a location within a shard. You can request that the iterator provide access to the oldest point, the newest point, or a particular point in the stream.
- `GetRecords` — Returns the stream records from within a given shard. You must provide the shard iterator returned from a GetShardIterator request.

# UE Resources
- https://www.macrometa.com/event-stream-processing/dynamodb-streams
