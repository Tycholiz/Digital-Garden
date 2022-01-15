
Base64 is an encoding scheme for translating text to binary, and back again. The resulting binary represents the binary data (a sequence of 8-bit bytes)

Base64 is designed to carry data stored in binary formats across channels that only reliably support text, not binary.

Base64 is prevalent in the World Wide Web.
- it is commonly used to embed image files (and other binary assets) within textual assets like HTML and CSS files

Base64 has some key differences with hashing. Notably, if you take the string "I like dogs" and hash it with a cryptography function like the one Bcrypt uses, you will get a completely different hash than if you change a single character in the string, for instance to "I like dogs!". If we are talking Base64 encoding however, the resultant strings will be almost the same. Observe that the strings are the same, except for the end, which is precicely where we find the `!`:
- "I like dogs" -> "SSBsaWtlIGRvZ3M="
- "I like dogs!" -> "SSBsaWtlIGRvZ3Mh"

### Example
Taking the following string
> Many hands make light work.

When the quote is encoded into Base64, it is represented as a byte sequence of 8-bit-padded ASCII characters encoded in MIME's Base64 scheme:
> TWFueSBoYW5kcyBtYWtlIGxpZ2h0IHdvcmsu

the encoded value of `Man` is `TWFu`
- Encoded in ASCII, the characters `M`, `a`, and `n` are stored as the byte values `77`, `97`, and `110`, which are the 8-bit binary values `01001101`, `01100001`, and `01101110`. These three values are joined together into a 24-bit string, producing `010011010110000101101110`.
