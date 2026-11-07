
Base64 is an encoding scheme for translating binary to and from text. The resulting binary represents the binary data (a sequence of 8-bit bytes)

Base64 encodes binary data in a printable ASCII string format. 
- It is commonly used when there is a need to transmit binary data over a communication medium that does not correctly handle binary data and is designed to deal with textual data only.

Base64 is designed to carry data stored in binary formats across channels that only reliably support text, not binary.

Base64 is prevalent in the World Wide Web.
- it is commonly used to embed image files (and other binary assets) within textual assets like HTML and CSS files

We use Base64 to transfer data over the network so that we don't run into nasty garbled text issues related to character encoding (e.g. `qÃ®Ã¼Ã¶:Ã`) 
- This problem happens because different computers across the world are handling strings in different character encodings, such as for example `UTF-8`, `ISO 8859-1`, etc.

Base64 has some key differences with hashing. Notably, if you take the string "I like dogs" and hash it with a cryptography function like the one Bcrypt uses, you will get a completely different hash than if you change a single character in the string, for instance to "I like dogs!". If we are talking Base64 encoding however, the resultant strings will be almost the same. Observe that the strings are the same, except for the end, which is precicely where we find the `!`:
- "I like dogs" -> "SSBsaWtlIGRvZ3M="
- "I like dogs!" -> "SSBsaWtlIGRvZ3Mh"

In cases where you encounter JSON parsing errors or unexpected behavior with payloads, especially when they contain binary or special characters, using base64 encoding can be a reliable solution to ensure data integrity during transmission and processing.

### Example
Taking the following string
> Many hands make light work.

When the quote is encoded into Base64, it is represented as a byte sequence of 8-bit-padded ASCII characters encoded in MIME's Base64 scheme:
> TWFueSBoYW5kcyBtYWtlIGxpZ2h0IHdvcmsu

the encoded value of `Man` is `TWFu`
- Encoded in ASCII, the characters `M`, `a`, and `n` are stored as the byte values `77`, `97`, and `110`, which are the 8-bit binary values `01001101`, `01100001`, and `01101110`. These three values are joined together into a 24-bit string, producing `010011010110000101101110`.
