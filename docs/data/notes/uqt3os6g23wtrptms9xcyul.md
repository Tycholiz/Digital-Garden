
### `put`
`put` is used to create and update documents

To update a document, we must provide the entire document along with its current revision marker (`_rev`).
- if we fail to do this, we will get HTTP 409
- therefore, to update a document we first need to fetch it so we can provide its `_rev` value to the `put` method.

### `allDocs`
`allDocs` reads many docs for us at once.
- it also allows you to reverse the order, filter by _id, slice and dice using "greater than" and "less than" operations on the _id, and much more.
- most of the time we should be using `allDocs`, not `query` 

For 99% of your applications, you should be able to use `allDocs()` for all the pagination/sorting/searching functionality that you need.

### `query`
The `query` API corresponds to the `_view` API of CouchDB.
- therefore `query` uses map/reduce functions

To use this API, we make a [[design doc|couchdb.design-doc]] and query it

Prefer `allDocs` or `changes` APIs when possible.