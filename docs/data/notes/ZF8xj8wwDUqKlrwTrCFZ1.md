
Relational databases assume that the relationships are of similar importance, document databases assume that relationships form a hierarchical structure and relationships between documents are less important

If your data cannot be represented on literally a sheet of paper, NoSQL is the wrong data store for you. And I don't mean sheets of paper with references that say "now turn to page 64 for the diagram", no, I mean a sheet of paper per document. That is what a normalized record looks like in a document store.

Horizontal scaling is a distinct benefit of NoSQL, which is why companies like Netflix and Spotify use document databases.
- RDBMSs more lend themselves to vertical scaling, which can get costly.

NoSQL databases fit better into the whole paradigm of distributed computing, and NoSQL databases make the most of cloud computing and storage. Cloud-based storage is an excellent cost-saving solution but requires data to be easily spread across multiple servers to scale up. Using commodity (affordable, smaller) hardware on-site or in the cloud saves you the hassle of additional software, and NoSQL databases like Cassandra are designed to be scaled across multiple data centers out of the box, without a lot of headaches.

Generally, NoSQL databases sacrifice ACID compliance for scalability and processing speed

Going from SQL to NoSQL is easier than from NoSQL to SQL

When all the other components of our application are fast and seamless, NoSQL databases prevent data from being the bottleneck.
- Big data is contributing to a large success for NoSQL databases, mainly because it handles data differently than the traditional relational databases.

# Types of NoSQL Databases
## Document-Based Databases
Document-based databases store the data in JSON objects. Each document has key-value pairs like structures:

The document-based databases are easy for developers as the document directly maps to the objects as JSON is a very common data format used by web developers. They are very flexible and allow us to modify the structure at any time.

Ex. Mongo, Couch, Couchbase

## Key-Value Database
Here, keys and values can be anything like strings, integers, or even complex objects. They are highly partitionable and are the best in horizontal scaling. They can be really useful in session oriented applications where we try to capture the behavior of the customer in a particular session.

key-value stores, in general, always maintain a certain number of replicas to offer reliability.

Ex. DynamoDB, Redis, Cassandra

## Wide Column-Based Database
This database stores the data in records similar to any relational database but it has the ability to store very large numbers of dynamic columns. It groups the columns logically into column families.
- For example, in a relational database, you have multiple tables but in a wide-column based database, instead of having multiple tables, we have multiple column families.
Cassandra or key-value stores, in general, always maintain a certain number of replicas to offer reliability.

Ex. Cassandra

# Implementations

## Cassandra
A key-value store approach to NoSQL

Cassandra's approach to data availability is as follows: Instead of having one master node, it utilizes multiple masters inside a cluster. With multiple masters present, there is no fear of any downtime. The redundant model ensures high availability at all times.

Cassandra is designed to manipulate huge data arrays across multiple nodes. 

In contrast to the relational database organizing data records in rows, Cassandra’s data model is based on columns to provide faster data retrieval. The data is stored in the form of hash.

designed to be scaled across multiple data centers out of the box, without a lot of headaches.

## DynamoDB (Amazon)
![[aws#dynamodb,1]]

## Elasticsearch
When to use ElasticSearch?
- If your use case requires a full-text search, Elasticsearch will be the best fit
- If your use case involves chatbots where these bots resolve most of the queries, such as when a person types something there are high chances of spelling mistakes. You can make use of the in-built fuzzy matching practices of the ElasticSearch
- Also, ElasticSearch is useful in storing logs data and analyzing it

## CouchDB
Like MongoDB, Couch is a document-oriented NoSQL databases, but Mongo and Couch diverge significantly in their implementations. 
- CouchDB uses the semi-structured JSON format for storing data. Queries to a CouchDB database are made via a RESTful HTTP API, using HTTP or JavaScript. 
- MongoDB uses BSON, a JSON variant that stores data in a binary format. MongoDB uses its own query language that is distinct from SQL, although they have some similarities. 

Like Mongo, Couch is schemaless.

CouchDB and MongoDB differ in their approach to the [[CAP theorem|deploy.distributed.CAP-theorem]] 
- CouchDB favors availability and partition tolerance
    - CouchDB uses eventual consistency. Clients can write to a single database node, and this information is guaranteed to eventually propagate to the rest of the database. 
- MongoDB prefers consistency and partition tolerance.
    - MongoDB uses strict consistency. The database uses a replica set to provide redundancy but at the cost of availability. 

As of this writing, Google projects the cost of deploying CouchDB on GCP at $34.72 per month. This estimate is based on a 30 day, 24 hours per day usage in the Central US region, a VM instance with 2 vCPUs and 8 GB of memory, and 10GB of a standard persistent disk.

## Couchbase
Every Couchbase node consists of a data service, index service, query service, and cluster manager component. Starting with the 4.0 release, the three services can be distributed to run on separate nodes of the cluster if needed.

In the parlance of CAP Theorem, Couchbase is typically run as a CP system (consistency & partition tolerant)

Provides a SQL-like query language called `N1QL` for manipulating JSON data stored in Couchbase.

## PouchDB
PouchDB was created to help web developers build applications that work as well offline as they do online.
It enables applications to store data locally while offline, then synchronize it with CouchDB and compatible servers when the application is back online, keeping the user's data in sync no matter where they next login.
Inspired by Couch
