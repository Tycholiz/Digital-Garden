
A streaming architecture is a defined set of technologies that work together to handle stream processing, which is the practice of taking action on a series of data at the time the data is created.

While streaming is an architectural-level concept, there are multiple ways it could be implemented. Most commonly, we can use [[pubsub|general.patterns.messaging.pubsub]], but we can also use source/sink.
- common sources and sinks are [[Kafka|kafka]], Hadoop

Streaming data refers to data that is continuously generated, usually in high volumes and at high velocity

A streaming data source would typically consist of a stream of logs that record events as they happen – such as a user clicking on a link in a web page, or a sensor reporting the current temperature.

ex. IoT sensors, Server and security logs, Real-time advertising, Click-stream data from apps and websites

In all of these cases we have end devices that are continuously generating thousands or millions of records, forming a data stream – unstructured or semi-structured form, most commonly JSON or XML key-value pairs.

Whenever you build a streaming based architecture, you have to account for anti-entropy in your streams.
- You have to consider that the sources of data can be unreliable, causing the stream to vary in bandwidth. You have to account for outages in any of the services you're connected to.
	- This is usually a problem in cases where we are counting on a fully up-to-date state of the world. In cases like the Facebook News feed, we don't really have this functional requirement.

## four key building blocks:
### The Message Broker / Stream Processor
This is the element that takes data from a source, called a producer, translates it into a standard message format, and streams it on an ongoing basis. Other components can then listen in and consume the messages passed on by the broker.

### Batch and Real-time ETL tools
Data streams from one or more message brokers need to be aggregated, transformed and structured before data can be analyzed with SQL-based analytics tools

### Data Analytics / Serverless Query Engine
After streaming data is prepared for consumption by the stream processor, it must be analyzed to provide value.
* * *

### Buffer
anal: Imagine adding various amounts of liquid to a big funnel. If we add liquid slowly, or fast, it doesn't change the rate at which the liquid exits the funnel. This is in essence what a buffer does for us. It is an intermediary data object that sits between the server and the client, and receives data at the rate that it is sent by the server.

* * *

Data can be processed as unbounded or bounded streams.
- Unbounded streams have a start but no defined end. They do not terminate and provide data as it is generated. Unbounded streams must be continuously processed, i.e., events must be promptly handled after they have been ingested. It is not possible to wait for all input data to arrive because the input is unbounded and will not be complete at any point in time. Processing unbounded data often requires that events are ingested in a specific order, such as the order in which events occurred, to be able to reason about result completeness.
- Bounded streams have a defined start and end. Bounded streams can be processed by ingesting all data before performing any computations. Ordered ingestion is not required to process bounded streams because a bounded data set can always be sorted. Processing of bounded streams is also known as batch processing.

# UE Resources
[Kappa Architecture: Arch for processing streaming data](https://hazelcast.com/glossary/kappa-architecture/)
- in use by Twitter
