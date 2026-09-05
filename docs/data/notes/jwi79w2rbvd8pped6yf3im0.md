
Large swathes of the web platform are built on streaming data
- that is, data that is created, processed, and consumed in an incremental fashion, without ever reading all of it into memory.
- the web has the Streams Standard which provides a common set of APIs for creating and interfacing with such streaming data, embodied in *readable streams*, *writable streams*, and *transform streams* (which consist of both a readable stream and a writable stream).
  - these APIs are designed to efficiently map to low-level I/O primitives
  - they allow us to efficiently compose streams together into *pipe chains*

Streams are primarily used by piping them to each other.

## Source
### Underlying Source
The underlying source is a lower-level I/O source. Most readable streams will wrap the underlying source.

There are two types of underlying source: *push sources* and *pull sources*. 

#### Push Source
Push sources push data at you, whether or not you are listening for it. 
- They may also provide a mechanism for pausing and resuming the flow of data. 
- ex. a [[TCP|protocol.TCP]] socket, where data is constantly being pushed from the [[os]] level, at a rate that can be controlled by changing the TCP window size.

#### Pull Source
Pull sources require you to request data from them. 
- The data may be available synchronously 
  - e.g. if it is held by the operating system’s in-memory buffers, or asynchronously, 
  - e.g. if it has to be read from disk. 
- ex. a file handle is a pull source, where you seek to specific locations and read specific amounts.

### Readable Streams
A readable stream represents a source of data, from which you can read (ie. data comes out of a `ReadableStream` instance)

A readable stream can be piped directly to a writable stream, using its `pipeTo()` method, or it can be piped through one or more transform streams first, using its `pipeThrough()` method.

Readable streams are designed to wrap both *push sources* and *pull sources* behind a single, unified interface.

* * *

## Sink
### Underlying Sink
Analogously to readable streams, most writable streams wrap a lower-level I/O sink, called the underlying sink. 
- Writable streams work to abstract away some of the complexity of the underlying sink, by queuing subsequent writes and only delivering them to the underlying sink one by one.

Code that writes into a writable stream using its public interface is known as a producer.

### Writable Stream
A writable stream represents a destination for data, into which you can write (ie. data goes into a `WritableStream` instance).

* * *

### Chunk
A chunk is a single piece of data that is written to or read from a stream.

It can be of any type; streams can even contain chunks of different types.

A chunk will often not be the most atomic unit of data for a given stream
- ex. a byte stream might contain chunks consisting of 16 KiB `Uint8Arrays`, rather than single bytes.

### Pipe chain
Once a pipe chain is constructed, it will propogate signals giving information as to how fast chunks should flow through it. If any step in the chain cannot yet process more chunks, it will propogate a signal backwards through the pipe chain until it eventually reaches the original source, which is told to stop producing chunks so fast. This process is called *backpressure*.