
A streaming architecture is a defined set of technologies that work together to handle [[stream processing|general.patterns.streaming]], which is the practice of taking action on a series of data at the time the data is created.

While streaming is an architectural-level concept, there are multiple ways it could be implemented. Most commonly, we can use [[pubsub|general.patterns.messaging.pubsub]], but we can also use source/sink.
- common sources and sinks are [[Kafka|kafka]], Hadoop

Whenever you build a streaming based architecture, you have to account for anti-entropy in your streams.
- You have to consider that the sources of data can be unreliable, causing the stream to vary in bandwidth. You have to account for outages in any of the services you're connected to.
	- This is usually a problem in cases where we are counting on a fully up-to-date state of the world. In cases like the Facebook News feed, we don't really have this functional requirement.

## Consuming a stream
Broadly, there are 3 things we can do with the events of a stream once we have it:
1. You can take the data in the events and write it to a database, cache, search index, or similar storage system, from where it can then be queried by other clients.
  - this is a good way of keeping a database in sync with changes happening in other parts of the system—especially if the stream consumer is the only client writing to the database.
2. You can push the events to users in some way
  - ex. by sending email alerts or push notifications, or by streaming the events to a real-time dashboard where they are visualized. 
    - In this case, a human is the ultimate consumer of the stream.
3. You can process one or more input streams to produce one or more output streams. Streams may go through a pipeline consisting of several such processing stages before they eventually end up at an output (option 1 or 2).


## four key building blocks:
### The Message Broker / Stream Processor
This is the element that takes data from a source, called a producer, translates it into a standard message format, and streams it on an ongoing basis. Other components can then listen in and consume the messages passed on by the broker.

### Batch and Real-time ETL tools
Data streams from one or more message brokers need to be aggregated, transformed and structured before data can be analyzed with SQL-based analytics tools

### Data Analytics / Serverless Query Engine
After streaming data is prepared for consumption by the stream processor, it must be analyzed to provide value.

## DSMS (Data Stream Management System)
While traditional databases have [[DBMSs|db.DBMS]], streaming architectures use a DSMS (data stream management system). 

While a DBMS holds static data, a DSMS holds dynamic data, the true size of which cannot be known (since it is always changing)

A DBMS also offers a flexible query processing so that the information needed can be expressed using queries. a DSMS executes a continuous query that is not only performed once, but is permanently installed (and therefore continuously executed).
- Unlike queries executed in a traditional DBMS, which return a result and exit, queries executed in a DSMS do not exit, generating results continuously as new data become available.

While data in a DBMS is accessed randomly (ie. random access), data in a DSMS is accessed sequentially

One of the biggest challenges for a DSMS is to handle potentially infinite data streams using a fixed amount of memory and no random access to the data. 
- there are 2 broad ways to limit the amount of data that needs to be processed at any given time (ie. processing step)
  1. Synopses - Take the raw data and compress it. This “compression” may be through sampling (ie. taking random data points to represent the whole picture). It may also be through calculating a running average of the values that come through the stream, since to calculate an average we only need the total members, along with the current average.
    - naturally, this doesn’t give us accurate results. 
  2. Window - only look at a portion of the data at a given time. This approach is motivated by the idea that only the most recent data are relevant. Therefore, a window continuously cuts out a part of the data stream, e.g. the last ten data stream elements, and only considers these elements during the processing. 
    - there are different types of windows, such as sliding windows (similar to FIFO), and tumbling windows. Windows can also be element-based, such as considering only the last 10 elements, or time-based, such as considering only the most recent 10 seconds of data.

Most query languages for a DSMS are based on SQL, but are not standardized, and thus vary from implementation to implementation.

* * *


# UE Resources
- [Kappa Architecture: Arch for processing streaming data](https://hazelcast.com/glossary/kappa-architecture/)
  - in use by Twitter
