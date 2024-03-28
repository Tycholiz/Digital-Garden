
A Buffer is an in-memory collection of raw bytes.
- The `Buffer` class lets us access these spaces of memory, thereby allowing us to work with binary data.

a `Buffer` is similar to an array of integers, but corresponds to a raw memory allocation outside the V8 heap.
- Unlike arrays, you cannot change the size of a buffer once it is created.

Because computers store data in bytes, a simple way to think of a Buffer is to think of it as an array of bytes:
```js
const buffer = [11111111, 11011001, 10010001]
```

Buffers are useful when you’re interacting with binary data, usually at lower networking levels.

The `Buffer` class can set its encoding so we can put a `String` into it. This enables it to translate from `String` of UTF16 characters into an array of bytes. Once you have a `String` in byte form you can use it in computing communication.
- In Javascript, a `String` is a collection of characters in UTF16 encoding.
  - Under UTF16 encoding a single character may consist of multiple bytes (at least one, usually not more than four)

When we read from a file with `fs.readFile()`, the data returned to the Promise (or callback) is a `buffer` object.
When we make HTTP requests in Node, they return data streams that are temporarily stored in an internal buffer when the client cannot process the stream all at once.

#### Encoding
Node supports the following encodings: ASCII, UTF-8, UTF-16, USC-2, Base64, Hexadecimal, binary (ISO/IEC 8859-1)
