
A stream is a way for us to incorporate buffering into the process of writing files
- We may add multiple avenues for this data to go in, releasing us from the restriction of only being able to insert one set of data in a single file
	- This gives the ability for a stream to be able to `pipe` its output to both stdout and a file

There are 4 types of streams in Node.js:
1. **Writable**: streams to which we can write data. For example, fs.createWriteStream() lets us write data to a file using streams.
2. **Readable**: streams from which data can be read. For example: fs.createReadStream() lets us read the contents of a file.
3. **Duplex**: streams that are both Readable and Writable. For example, net.Socket
4. **Transform**: streams that can modify or transform the data as it is written and read. For example, in the instance of file-compression, you can write compressed data and read decompressed data to and from a file.

In a Node.js based HTTP server, `request` is a readable stream and `response` is a writable stream
The fs module lets you work with both readable and writable file streams

Whenever you’re using Express you are using streams to interact with the client, also, streams are being used in every database connection driver that you can work with, because of TCP sockets, TLS stack and other connections are all based on Node.js streams.

### How to create a readable stream
We first require the Readable stream, and we initialize it.
```js
const Stream = require('stream')
const readableStream = new Stream.Readable()
```

Now that the stream is initialized, we can send data to it:
```js
readableStream.push('ping!')
readableStream.push('pong!')
```

#### Async iterator (`for await`)
[[js.lang.feat.async-iterator]]

#### Pipe vs. Write
`pipe` is like positioning the pipes that will change the flow of water, before anything has actually gone through those pipes. `write` is like turning on the water.

### Built-in streams
a request to an HTTP server and process.stdout are both stream instances.
All streams are instances of EventEmitter.

# UE Resources
[Async Iterator](https://nodesource.com/blog/understanding-streams-in-nodejs/)
