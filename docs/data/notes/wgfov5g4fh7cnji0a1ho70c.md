
JSON Patch is a format for describing changes to a JSON file
- think of it like a patchfile in Git: instead of sending over the entire repo with a single change, we send a diff that describes the changes that we've made.

JSON Patch can be used in combination with [[HTTP Patch|protocol.http.methods#patch,1:#*]] methods to perform partial updates

A JSON Patch document is just a JSON file containing an array of patch operations. 
- The patch operations supported by JSON Patch are `add`, `remove`, `replace`, `move`, `copy` and `test`. 
- The operations are applied in order: if any of them fail then the whole patch operation should abort.

The original document
```json
{
  "baz": "qux",
  "foo": "bar"
}
```

The patch
```json
[
  { "op": "replace", "path": "/baz", "value": "boo" },
  { "op": "add", "path": "/hello", "value": ["world"] },
  { "op": "remove", "path": "/foo" }
]
```

The result
```json
{
  "baz": "boo",
  "hello": ["world"]
}
```