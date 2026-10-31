
## What is it?
CouchDB is a [[document database|db.type.document]], with each document represented as JSON. 

The documents are organised via [[views|couchdb.design-doc.view]]. 
- Views are defined with aggregate functions and filters are computed in parallel, much like MapReduce.

Each database is a collection of independent documents. Each document maintains its own data and self-contained schema. An application may access multiple databases, such as one stored on a user's mobile phone and another on a server. Document metadata contains revision information, making it possible to merge any differences that may have occurred while the databases were disconnected.

Unlike other solutions, `databases` in CouchDB are cheap to make. They are more comparable to namespaces in other database systems.

CouchDB was designed with sync in mind, and this is exactly what it excels at. Many of the rough edges of the API serve this larger purpose
- unlike other common [[replication|db.distributed.replication]] systems, CouchDB doesn't use leader-follower replication— it uses multi-master, meaning any node can be read from and written to.
- By default, CouchDB (and [[PouchDB|couchdb.pouch]]) are designed to store all document revisions forever, just like [[git]]

### From SQL to CouchDB
| SQL                    | CouchDB                      |
|------------------------|------------------------------|
| Queries                | HTTP requests with JSON      |
| Each table has rows    | Each database has documents  |
| Each table has columns | Each document has fields     |

CouchDB config files are found in `ini`, and there are multiple [options](https://docs.couchdb.org/en/3.2.0/config/intro.html#configuration-files) for location.

## Why use it?
CouchDB is well suited for applications with accumulating, occasionally changing data, on which pre-defined queries are to be run and where versioning is important (e.g. CRMs, [[CMSs|CMS]]). 
- Master-master replication is an especially interesting feature, allowing easy multi-site deployments.

CouchDB is really good at [[replication|couchdb.replication]] and [[scalability|deploy.scaling]]
- Couchdb implements [[multi-master replication|db.distributed.replication.strategies#multi-leader-based-replication,1:#*]]
- Couchdb is designed with bi-directional replication (or synchronization) and off-line operation in mind. 
    - That means multiple replicas can have their own copies of the same data, modify it, and then sync those changes at a later time.

Couchdb offers document-level ACID semantics by implementing a form of Multi-Version Concurrency Control, meaning that CouchDB can handle a high volume of concurrent readers and writers without conflict.

CouchDB guarantees eventual consistency to be able to provide both availability and partition tolerance.

### Conflict resolution
instead of locks, Couchdb uses [[MVCC|db.strategies.concurrency-control#multiversion-concurrency-control-mvcc,1]]

Conflict resolutions are automatic and deterministic (since the contents of the document are hashed with MD5 and sorted alphanumerically)
- anal: this is like a CPU-cheap version of blockchain, but instead of solving some complex mathematical formula to prove your conflict is the correct one, a simple MD5 formula is used to make that determination.
- Custom (and even manual) resolution can be built into the app if we don't want to do this by sorting alphanumeric MD5 sums.
- In either case, both revisions are preserved.
    - actually "deleting" is only soft deleting in CouchDB, with a field of `_deleted`.

Each document in CouchDB is versioned (via its `_rev` field)
- the `_rev` is prefixed with an integer that increments with each new revision.
- if 2 people update the same document at the same time, then we will end up with two revisions prefixed with the next incremented number:
    - `2-1583746` and `2-2583368`. In this case, the first would win, because of its alphanumerical position.

Resolving a conflict generally involves first merging data into one of the documents, then deleting the stale one.

Couch uses MVVC instead of locks.

### Resources
`http://<host>:<port>/<database>/<document>/<attachments>`

System endpoints/keys are prefixed with `_`

Fauxton dashboard: `http://localhost:5984/_utils`

### Querying data
There are 3 ways to query data: MapReduce, Mango queries, and Full-text search (with Lucene).

Mango queries are meant to resemble the MongoDB query language.
- allows us to do arbitrary searches with `=`, `>`, [[regex]] etc.
- if we use Mango queries, the view is created for us automatically.

### Security
CouchDB's level of security is not as sophistocated as other database solutions.
- security is per database (via the `_security` endpoint), and we can specify read-only access, read/write access, or admin access.
    - admins can update indexes, views etc.

If finer grained control is needed, we can write custom `update` functions to implement our own business logic for determining when data should be accessible or not.

* * *

CouchDB and PouchDB do not support transactions. A document is the smallest unit of operations.

## Hosting
- A2 Hosting
- Cloudant (IBM managed CouchDB)