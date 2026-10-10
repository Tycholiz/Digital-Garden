
PouchDB is a Javascript implementation of CouchDB which is API compatible with it. 
- So you can use [[CouchDB|couchdb]] on the server side and Pouch in the application itself and once the application comes online you can sync both.
- the replication algorithm in PouchDB is identical to the one in CouchDB.

Since Couchbase and Cloudant (both descendents of Couchdb) share the same sync protocol, they can be used with PouchDB.

### The nature of PouchDB
PouchDB itself is not a database

PouchDB databases can be either remote or local
- When creating a local database, whatever datastore that is available is used:
  - IndexedDB for most browsers
  - LevelDB for Node.js
- When creating a remote PouchDB database, it communicates directly with the remote database (CouchDB, Couchbase, Cloudant)

The goal of PouchDB is that we can communicate with either the local or remote database and we shouldn't notice a difference in how we interact with each one.

PouchDB has two types of data: documents and attachments.
- attachments are the most efficient way to story binary data (e.g. mp3, jpg)

### Plugins
PouchDB is built in a modular way. We can pick and choose which modules we need from PouchDB, and in turn reduce the bundle size.
- PouchDB packages come in three flavors: presets, plugins, and utilities.
  - Presets are a collection of plugins, which expose a PouchDB object that is ready to be used.
    - ex. pouchdb-browser exists to provide a certain combination of modules that can be expected for use of PouchDB in the browser. Specifically, this means it ships with the IndexedDB adapter as its default adapter, and also contains the replication, HTTP, and map/reduce plugins.
  - Plugins are features that can be added to a PouchDB instance using the PouchDB.plugin() API.
  - Utilities are grab-bags of helper functions, and are only recommended for advanced use cases.


### Adapters
Since PouchDB is just an abstraction layer that implements the same interface as CouchDB, we need an underlying "sourcing layer" (e.g. database, REST resource via HTTP) to handle retrieval for us. To connect the PouchDB interface to the underlying data layer of our choice, we must use an [[adapter|general.patterns.structural.adapter]]
- by default, PouchDB ships with the IndexedDB adapter for the browser and LevelDB for Node.js, but we can use different underlying data layers such as SQLite
- we can even use an HTTP adapter if we want to use a remote CouchDB instance as our data layer. This would effectively be like saying "I want to just use the PouchDB abstraction layer instead of me having to interact with my CouchDB instance directly".

* * *

`seq` can be thought of as a version number for the entire database. Basically it answers the question of "How many total changes have been made to all documents in this database?" This sets it apart from the revision hash `_rev`, which marks the changes made to a single document.
- the `seq` between two databases is not guaranteed to be kept in sync. CouchDB and PouchDB have slightly different ways of increasing their `seq` values, so really `seq` is only meaningful within a single database.


## Plugins 
- [Relational Pouch](https://github.com/pouchdb-community/relational-pouch)
    - allows you to interact with PouchDB/CouchDB like a relational data store, with types and relations.
    - includes an API for many-to-many relationships

## E Resources
- [Roving engineer problem](https://medium.com/@glynn_bird/replicating-from-a-query-with-couchdb-dd3ffc5c4b31#:~:text=The%20one%20database%20per%20user,mobile%20clients%20replicate%20to%2Ffrom.)