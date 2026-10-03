
To make an app truly offline first, we primarily need to do two things:
1. Any code and assets used should be available offline
2. Any data changes should be made locally first and then [[synced|db.distributed.sync]] to the cloud.

There are 2 categories of offline-first app that has different implications for the app's architecture: single-user and multi-user.
- single-user is an application like Everdo, where there is only ever one user editing a document at a time
- multi-user is an application like Trello or GoogleDocs, where multiple users can edit the same document at once
  - multi-user applications may benefit more from a data structure like [[CRDTs|general.lang.data-structs.CRDT]]

When the backend isn't very smart it's not too hard, you can just encapsulate any state-changing action into an event, when offline process the events locally + cache the chain of events, when online send the chain to the backend and have the backend apply the events one by one. Periodically sync new events from the backend and apply them locally to stay in sync. This is what classic Todo apps like OmniFocus do.
- The problems start when the backend is smarter and also applies business logic that generates new events (for example enriching new entities with data from external systems, or adding new tasks to a timeline etc). Naturally, the new server-generated events are only available later when the client comes back online.

### Offline vs Cloud apps
In cloud apps, the data on the server is treated as the primary, authoritative copy of the data; if a client has a copy of the data, it is merely a cache that is subordinate to the server. Any data modification must be sent to the server, otherwise it “didn't happen.” 

In offline-first applications we swap these roles: we treat the copy of the data on your local device — your laptop, tablet, or phone — as the primary copy. Servers still exist, but they hold secondary copies of your data in order to assist with access from multiple devices. 

Offline-first apps can use end-to-end encryption so that any servers that store a copy of your files only hold encrypted data that they cannot read.

## Tools
- [WatermelonDB](https://github.com/Nozbe/WatermelonDB)
- [RxDB](https://rxdb.info/)
- [CouchDB+PouchDB](https://medium.com/offline-camp/couchdb-pouchdb-and-hoodie-as-a-stack-for-progressive-web-apps-a6078a985f18)
- [Dexie](https://github.com/dexie/Dexie.js)

## E Resources
- [myTunes: using SQLite to create a reactive app](https://riffle.systems/essays/prelude/)
- [local-first apps](https://www.inkandswitch.com/local-first/)
- [Downsides of offline-first](https://rxdb.info/downsides-of-offline-first.html)