
Views are the primary tool used for querying and reporting on CouchDB databases.
- we write some Javascript functions which combine to create a view. This code implements the business logic for retrieving the documents. 
- The view gets created as an object in the database, which is exposed as a resource at an HTTP endpoint.

Views also allow us to build [[indexes|db.strategies.index]] on any document value or structure.

views are basically highly efficient on-disk dictionaries that map keys to values
- the key is automatically indexed and can be used to filter and/or sort the results you get back from your views. 
  - in fact the results are always sorted by the key. This fact should be leveraged to get the most out of queries. 
  - This should be the approach taken whenever we would use `SORT BY` in SQL. It can be achieved relatively easily by using an array as a key, 
- When you "invoke" a view, you can say that you’re only interested in a subset of the view rows by specifying a `?key=foo` query string parameter.

Stored data is structured using views. In CouchDB, each view is constructed by a JavaScript function that acts as the Map half of a map/reduce operation. The function takes a document and transforms it into a single value that it returns. CouchDB can index views and keep those indexes updated as documents are added, removed, or updated.

Views are generally stored in the database and their indexes updated continuously.
- external servers can also be used to store views. Implementations exist in JavaScript, Python, Ruby etc

Each view you create corresponds to one [[B-tree|general.lang.data-structs.tree.B]]
- The B-trees used are an *append-only*/*copy-on-write* variant that does not overwrite pages of the tree when they are updated, but instead creates a new copy of each modified page.

All views in a single design document will live in the same set of index files on disk

A view is just like any other document with a key difference: the `_id` is prefixed with `_design/`

The most practical consideration for separating views into separate documents is how often you change those views. 
- Views that change often, and are in the same design document as other views, will invalidate those other views’ indexes when the design document is written, forcing them all to rebuild from scratch. Naturally we want to avoid this in production.

When you have multiple views with the same map function in the same design document, CouchDB will optimize and only calculate that map function once.
- This lets you have two views with different reduce functions (say, one with _sum and one with _stats) but build only a single copy of the mapped index.

Generating a view is an `0(N)` operation (where `N` is the total number of rows in the view).

## View functions
View functions specify a key and a value to be returned for each row.

Basically, the idea with map/reduce functions is that you divide your query into a map function and a reduce function, each of which may be executed in parallel in a multi-node cluster.
- this allows us to perform efficient query operations in parallel

### Map function
Take in a single document and `emit()` key-value pairs that are stored in a View.

A map function is free from side-effects.

The following `map` function will take in a single `nugget` and will emit a key-value pair for each item in the `buckets` array.
- `emit()` may be called many times for a single document, so the same document may be available by several different keys.
```js
function (doc) {
  if (doc.type === 'nugget' && doc.buckets && Array.isArray(doc.buckets)) {
    doc.buckets.forEach(function (bucket) {
      emit(bucket.toLowerCase(), 1);
    });
  }
}
```

If you don’t use the key field in the map function, you are probably doing it wrong.

CouchDB is smart enough to run a map function only once for every document, even on subsequent queries on a view. Only changes to documents or new documents need to be processed anew.

Map functions can't modify the document and they can't have side-effects.
- this is why CouchDB can guarantee correct results without having to recalculate a complete result when only one document gets changed.

### Reduce function
Reduce functions operate on the sorted rows emitted by a map function

A reduce function takes in 2 args: a key, and a list (which is the result of the related map function)
- it can also take an optional 3rd arg, which is to indicate whether *rereduce* mode is active or not.

While the map function will `emit` values, `reduce` will simply `return` the reduced value.

A reduce function should return a scalar value (ie. not an array or object)
- reduce is not for generating complex aggregate values
- it is ok to return an array if it is small and fixed size (ie. more of a tuple)

CouchDB has a number of built-in reduce functions which run inside the DB and are much faster than the equivalent implementation in a custom reduce function (in Javascript)
- `_sum` - returns total number of rows between startkey and endkey
- `_count`
- `_stats`

Because of the way B-trees are structured, we can cache the intermediate reduce results in the non-leaf nodes of the tree, so reduce queries can be computed along arbitrary key ranges in logarithmic time.

CouchDB’s reduce functionality takes advantage of one of the fundamental properties of B-tree indexes: for every leaf node (a sorted row), there is a chain of internal nodes reaching back to the root. Each leaf node in the B-tree carries a few rows (on the order of tens, depending on row size), and each internal node may link to a few leaf nodes or other internal nodes.

The reduce function is run on every node in the tree in order to calculate the final reduce value. The end result is a reduce function that can be incrementally updated upon changes to the map function, while recalculating the reduction values for a minimum number of nodes. The initial reduction is calculated once per each node (inner and leaf) in the tree.

You can access a view without enabling the reduce function by disabling reduction (`reduce=false`) when the view is accessed.

#### Rereduce
The existence and use of the rereduce parameter is tightly coupled to how the B-tree index works.

When run on leaf nodes (which contain actual map rows), `rereduce` is false.

When the reduce function is run on inner nodes, `rereduce` is true. This allows the function to account for the fact that it will be receiving its own prior output. When rereduce is true, the values passed to the function are intermediate reduction values as cached from previous calculations. When the tree is more than two levels deep, the rereduce phase is repeated, consuming chunks of the previous level’s output until the final reduce value is calculated at the root node.

## View result
Whenever you query a view, this is how CouchDB operates:
1. Starts reading at the top (of the B-tree), or at the position that `startkey` specifies, if present.
2. Returns one row at a time until the end or until it hits `endkey`, if present.
  - If you specify `descending=true`, the reading direction is reversed; the sort order of rows in the view doesn't change.

There is no unique constraint on the keys of a view result.
- ex. we can have a view result of K-V pairs `author`-`book` where the same author is listed multiple times.

You always need to give a range of keys, because filtering is done on map's results, not on reduce.

Each view result is stored in a B-tree (index structure) in its own file
- since view results are in their own file, they can be stored on a separate disk from other database objects to increase performance.
- The B-tree provides very fast lookups of rows by key, as well as efficient streaming of rows in a key range
- The B-tree is created only once when the view is first queried. 
  - All subsequent queries will just read the B-tree instead of executing the map function for all documents again.
  - When we create, update or delete a document, the database engine finds the corresponding rows in the view result and marks them *invalid* so they no longer show up in the view result.
    - if the document got updated, it gets run through the map function again and the result is inserted into the B-tree at the correct spot.

The results of a view are generated via the `emit()` function.

It is not recommended to emit the document itself in the view. 
- Instead, to include the bodies of the documents when requesting the view, request the view with `?include_docs=true`.

View results are sorted by *key*
- the key can be any data type, including arrays and objects.
  - You can use JSON arrays as keys for fine-grained control over sorting and grouping.

For instance, here we get back customers, then orders in the same view result (due to the `0` and `1`, serving as secondary sorts)
```js
function(doc) {
  if (doc.Type == "customer") {
    emit([doc._id, 0], null);
  } else if (doc.Type == "order") {
    emit([doc.customer_id, 1], null);
  }
}
```

Aside from the data itself, the view result also contains metadata like `total_rows` and `offset`.

## Collating Views
Imagine our querying client needs 2 different related objects in a single query. In SQL, we'd do a [[JOIN|sql.join]], but there are a few different approaches in CouchDB:

### Embed related data in one document
We could embed the related data like so:
```json
{
  "_id": "ABCDEF",
  "title": "my_nugget",
  "media_items": "…",
  "buckets": [
    {"title": "psychology"},
    {"title": "humor"}
  ]
}
```

However, a drawback is that to add a bucket, we need to
1. fetch the document (the nugget)
2. add the bucket to the `buckets` field
3. send the updated document to the server

To boot, if we get multiple clients adding buckets to the same nugget, we are bound to get *HTTP 409 Conflict* (optimistic concurrency in action)

### Keep nuggets and buckets separate
We could add a `type` field to separate nuggets and buckets:
```json
{
  "_id": "ABCDEF",
  "type": "nugget",
  "title": "my_nugget",
  "media_items": "…",
  "buckets": ["123456"]
}
```
```json
{
  "_id": "123456",
  "type": "bucket",
  "title": "my_bucket",
}
```

The problem with this approach is that if we want to get both the nugget and the associated buckets, we have to make 2 GET requests.

### Using view collation
Thankfully, we just have to write a view that leverages the fact that keys are automatically sorted, and can be complex:
- note: the `0` and `1` are needed so that the nugget comes first, then the buckets.
- note: this view is not totally tested
```js
function(doc) {
  if (type === "nugget") {
    emit([doc.bucket, 0], null)
  } else if (type === "bucket") {
    emit([doc._id, 1], null)
  }
}
```

With this view, we get back rows where the first one (with `key=[id, 0]`) is the nugget, and the rest are associated buckets. 

Now, to get back a specific nugget with all the associated nuggets, we add the query param to the query:
- `?startkey=["my_nugget"]&endkey=["my_nugget", 2]&include_docs=true`

To query by some specific value(s) in CouchDB, we just need to figure out how to get that searchterm in the key so we can use a query param on it.
- ex. imagine our document had a `$doctype` field and we wanted to return documents where that type is `nugget`:
```js
function(doc) {
  emit(doc.$doctype, null)
}
```
and then we just add a query param `?key="nugget"`

* * *

## Performance
The database engine only runs through all documents once, when you first query your view. 
- If a document is changed, the map function is only run once, to recompute the keys and values for that single document.

## Resources
- [Cookbook for SQL developers](https://docs.couchdb.org/en/3.2.0/ddocs/views/nosql.html)