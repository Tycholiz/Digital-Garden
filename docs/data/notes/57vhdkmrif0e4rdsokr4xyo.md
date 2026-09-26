
Design documents are used to build [[indexes|db.strategies.index]], validate document updates, format query results, and filter replications

Design documents contain functions such as view and update functions. These functions are executed when requested.

Design documents are denoted with an `_id` with the format `_design/my-design-document`

```json
{
  "_id": "_design/example",
  "views": {
    "view-number-one": {
      "map": "function (doc) {/* function code here - see below */}"
    },
    "view-number-two": {
      "map": "function (doc) {/* function code here - see below */}",
      "reduce": "function (keys, values, rereduce) {/* function code here - see below */}"
    }
  },
  "updates": {
    "updatefun1": "function(doc,req) {/* function code here - see below */}",
    "updatefun2": "function(doc,req) {/* function code here - see below */}"
  },
  "filters": {
    "filterfunction1": "function(doc, req){ /* function code here - see below */ }"
  },
  "validate_doc_update": "function(newDoc, oldDoc, userCtx, secObj) { /* function code here - see below */ }",
  "language": "javascript"
}
```

each separate design document will spawn another (set of) couchjs processes to generate the view, one per shard. 
- Depending on the number of cores on your server(s), this may be efficient (using all of the idle cores you have) or inefficient (overloading the CPU on your servers).