
### Unmarshaling
To read data from JSON received from over the internet, we need to Unmarshall it first
```go
json.Unmarshal(reqBody, &article)
```

Before we unmarshall the JSON, it is represented as an array of bytes.

### Marshal
`json.NewEncoder` and `json.Marshal` both marshal objects into JSON encoded strings.
- The difference being that the Encoder first marshals the object to a JSON encoded string, then writes that data to a buffer stream. The Encoder therefore, uses more code and memory overhead than the simpler `json.Marshal`.

- Encoding/decoding JSON refers to the process of actually reading/writing the character data to a string or binary form.
- Marshaling/Unmarshaling refers to the process of mapping JSON types from and to Go data types and primitives.

- In Go, struct data is converted into JSON using `Marshal()` and JSON data to string using `Unmarshal()` method.