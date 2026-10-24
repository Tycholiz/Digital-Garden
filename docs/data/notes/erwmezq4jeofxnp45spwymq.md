
[[see also CouchDB Replication|couchdb.replication]]

### Unidirectional Replication
With unidirectional replication, one database will mirror its changes to a second one, but writes to the second database will not propagate back to the master database.

```js
// replicate all changes from localDB to remoteDB
localDB.replicate.to(remoteDB).on('complete', function () {
  // yay, we're done!
}).on('error', function (err) {
  // boo, something went wrong!
});
```

### Bidirectional Replication
```js
localDB.replicate.to(remoteDB);
localDB.replicate.from(remoteDB);

// or, equivalently:
localDB.sync(remoteDB);
```

### Live Replication (a.k.a continuous replication)
With this mode, changes are propagated between the two databases as the changes occur. 
- In other words, normal replication happens once, whereas live replication happens in real time.
- however if the user goes offline, an error will be thrown and replication will stop.

```js
localDB.sync(remoteDB, {
  live: true
}).on('change', function (change) {
  // yo, something changed!
}).on('error', function (err) {
  // yo, we got an error! (maybe the user went offline?)
});
```