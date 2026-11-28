
Key-value storage, where:
- All key and value are of same type.
- provides fast lookup and values that can retrieve, update or delete with the help of keys.
- can iterate over keys (as long as they are indexed)
- Don’t need to know all the keys at compile time.

```go
// create a map with keys as strings and values as integers
var myMap = make(map[string]int)

// set value of key
myMap["firstKey"] = 2
```