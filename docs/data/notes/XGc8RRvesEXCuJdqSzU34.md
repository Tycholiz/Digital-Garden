
### Base64 encode JSON
```js
const valueEncoded = Buffer.from(JSON.stringify(value), 'base64')
```

### Creating a Buffer object
There are 2 ways to create a buffer object:
1. Create a new buffer object, allocating the size during instantiation
2. Extract a buffer from existing data

#### Create new buffer
If you are going to store data in memory that you have yet to receive, you’ll want to create a new buffer with `Buffer.alloc(1024)`

When creating a new buffer with `alloc()`, it is filled by default with binary zeroes to serve as a placeholder.

#### Extract a buffer
To create a buffer from pre-existing data, we use `Buffer.from()`
we can create buffers from:
- array of integers (between 0 and 255)
- an `ArrayBuffer` object, which stored a fixed length of bytes
- a string
- another Buffer

### Reading from a Buffer object
We can access an individual byte in a buffer or we can extract the entire contents.
- To access one byte of a buffer, we pass the index or location of the byte we want. Buffers store data sequentially like arrays. They also index their data like arrays, starting at 0. We can use array notation on the buffer object to get an individual byte.

The buffer object comes with the `toString()` and the `toJSON()` methods, which return the entire contents of a buffer in two different formats.

### Modify a Buffer
You can either modify buffer bytes individually using the array syntax, or write new contents to the buffer, replacing the existing data.

# E Resources
https://www.digitalocean.com/community/tutorials/using-buffers-in-node-js
