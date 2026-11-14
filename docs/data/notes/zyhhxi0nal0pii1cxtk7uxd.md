
Streaming data refers to data that is continuously generated, usually in high volumes and at high velocity

In general, a "stream" refers to data that is incrementally made available over time. 
- The concept appears in many places, such as... 
    - in the stdin and stdout of Unix
    - programming languages (lazy lists)
    - filesystem APIs (such as Java’s FileInputStream)
    - TCP connections
    - delivering audio and video over the internet

A streaming data source would typically consist of a stream of logs that record events as they happen – such as a user clicking on a link in a web page, or a sensor reporting the current temperature.

ex. IoT sensors, Server and security logs, Real-time advertising, Click-stream data from apps and websites

In all of these cases we have end devices that are continuously generating thousands or millions of records, forming a data stream – unstructured or semi-structured form, most commonly JSON or XML key-value pairs.

### Bounded vs Unbounded Streams
You can either organize your data processing around bounded or unbounded streams, and which of these paradigms you choose has profound consequences.

#### Unbounded streams 
With unbounded streams, we are using the *stream processing* paradigm.

- have a start but no defined end. 
- do not terminate and provide data as it is generated. 
- Unbounded streams must be continuously processed, 
    - i.e., events must be promptly handled after they have been ingested. It is not possible to wait for all input data to arrive because the input is unbounded and will not be complete at any point in time. 
- Processing unbounded data often requires that events are ingested in a specific order, such as the order in which events occurred, to be able to reason about result completeness.

#### Bounded streams 
In this mode of operation you can choose to ingest the entire dataset before producing any results
- this means we can do things like sort the data, compute global statistics, or produce a final report that summarizes all of the input.

When you process a bounded data stream, we are using the *batch processing* paradigm.

- have a defined start and end. 
- can be processed by ingesting all data before performing any computations. 
- Ordered ingestion is not required to process bounded streams 
    - this is because a bounded data set can always be sorted. 
- Processing of bounded streams is also known as batch processing.

### Buffer
anal: Imagine adding various amounts of liquid to a big funnel. If we add liquid slowly, or fast, it doesn't change the rate at which the liquid exits the funnel. This is in essence what a buffer does for us. It is an intermediary data object that sits between the server and the client, and receives data at the rate that it is sent by the server.
