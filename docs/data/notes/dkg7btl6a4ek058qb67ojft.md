
To use Change Streams, we switch to the collection (`use <collection>`) then open a cursor on a collection with:
```sh
let cursor = db['collection-name'].watch()
```

- we can also open it on a whole database with `db.watch()`

We poll for change events using:
- `cursor.hasNext()` - returns boolean
- `cursor.next()` - return the next change event (or error)
- `cursor.tryNext()` - returns the next change event (or null)

Change Event:
- `_id` - change eventId (not the `_id` of the document that was changeed)
  - this is used as a `ResumeToken`
- `operationType` - insert, delete, update, replace etc.
- `fullDocument` - the latest version of an entire document of insert, delete, update, and replace events.
  - if there are multiple changes on the same document over a period of time but none of those change events got processed in the intervening period, the `fullDocument` will still show the latest data, as opposed to the data we'd have as a result of any specific operation.
- `updateDescription` - fields that were updated or removed and the values of those fields after the change.

Change Streams are filterable, which is accomplished by defining an [[aggregation|mongo.aggregation]] pipeline.

Change Streams are resumable, which is accomplished by passing either ResumeTokens or timestamps (`resumeAfter`) when configuring the [[cursor|db.strategies.cursors]].
- A strategy is to store ResumeTokens in some data layer (e.g. another collection of the same Mongo database) so that the consumer of the stream knows from where to start polling.
  - ex. if we are using Lambda to consume events, the Lambda will have to know from where to resume polling, since Lambdas are ephemeral.

## Cook
All commands can be run from `mongo` shell

```js
let cursor = db.collection.myCollection.watch()
cursor.next()
```