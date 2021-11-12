
a `Buffer` is similar to an array of integers, but corresponds to a raw memory allocation outside the V8 heap.
```js
const buffer = [11111111, 11011001, 10010001]
```

The `Buffer` class can set its encoding so we can put a `String` into it. This enables it to translate from `String` of UTF16 characters into an array of bytes. Once you have a `String` in byte form you can use it in computing communication.
- In Javascript, a `String` is a collection of characters in UTF16 encoding. 
    - Under UTF16 encoding a single character may consist of multiple bytes (at least one, usually not more than four)