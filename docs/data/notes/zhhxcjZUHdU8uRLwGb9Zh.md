
`mongod` is the main daemon process. Think of it as the API of the Mongo Engine. It handles data requests, manages data access, and performs background management operations.
- A replica set is a group of `mongod` processes that maintain the same data set.
	- anal: spinning up multiple pods of the same REST API server. The reason for both is the same: to provide redundancy and high availability.

* * *

## Normalizing data
- `normalized` - when data is normalized, it means that instead of having documents called `Folders` that has a field called `files` (an array of actual objects— not just the references to the objects), we have documents that are completely separated from one another, and they just make reference to one another.
	- a.k.a Referencing and Embedding
- denormalized:
```js
folder = {
  name: "tagA",
  files: [
		{
			title: "Tut #1",
			author: "bezkoder"
		},
		{
			title: "Tut #2",
			author: "zkoder"
		}
	]
}
```
- normalized:
```js
folder = {
  name: "tagA",
  tutorials: ["238fnnc3", "edn38bsn39"]
}
```

### When to Normalize/Denormalize
if we were to break "one to many" relationships down, we could have:
	- *1:few* - ex. 1 movie has a few awards
	- *1:many* - ex. 1 movie has potentially 1000s of reviews
	- *1:tonnes* - ex. 1 chat app has millions of messages
- We need to distinguish between these relationships when we need to determine whether to normalize or denormalize
	- note: This granularity isn’t necessary in relational databases.

#### Deciding based on relationship type
- For *1:few*, we always use denormalized (embedded) data
- For *few:few* we can just embed
	- what about *few:many*?
- For *1:tonnes* and *many:many*, almost always use normalized (referenced) data
- *1:many* could go either way

#### Deciding based on how data will be interacted with
- If the data is mostly read and does not change quickly, we should probably embed
	- ex. photo gallery related to a movie: once we gather the photos, we probably won’t be updating them too frequently.
- If the data is updated frequently, we should probably reference
	- ex. user generated reviews and ratings of movies. Those vote counts are going to be changing all the time, and we don’t want to query the whole movie document every time a vote is cast. for this reason, it makes sense for reviews to be its own collection with its own documents that reference its parent
- Imagine Instahop, where each tour guide is also a user. If we were to embed the tourguide directly into the `tour` document, changing any fields in the guide's document would be a pain, since we'd have to update it both in the `tour` document and the `user` document
	- Child referencing would be best used here

- in Mongoose, `populate()` is how to denormalize the data as we are sending it to the client

## Referencing other collections
### Types of referencing
#### Child referencing (child ignorance/parent holds the child reference)
- best for *1:Few* relationships

#### Parent referencing (parent ignorance/child holds the parent reference)
- best for *1:Many*/*1:Tonnes* relationships

#### Two-way referencing
- best for *Many:Many* relationsips

- If we have a Nuggets collection and a Buckets collection, we have a many-to-many relationship between the two. We could Have a field in the Buckets model called `nuggets`, which would be an array of nugget ids. This would be sufficient for some cases, but what if we wanted to get a list of the buckets that a particular nugget belongs to, while we are looking at a nugget? To make this easier, we should have a field in the Nuggets model called `buckets`. This makes things easier in a way, but means that if we want to put the nugget in another bucket, we need to update both the document in the Buckets collection and the document in the Nuggets collection. In other words, using this schema design means we can no longer change the bucket field from the Nugget collection in a single atomic update.

## Sharded Cluster
- spec: instead of Mongo deploying a single instance of your database (on a single computer), your database is deployed on different machines (the cluster), and are sharded
	- sharded means that not all of the data related to the application will be stored in a single database. Multiple will be used, each one storing different data
- the `mongos` provides the interface between the client and the sharded cluster
	- From the perspective of the application, a mongos instance behaves identically to any other MongoDB instance
- MongoDB shards data at the collection level, distributing the collection data across the shards in the cluster.
	- In other words, the data of a collection is partitioned and stored in different clusters
- MongoDB uses a *Shard Key* to determine where the data should be split for storage across the different shards.
	- Therefore, the shard key consists of a field or multiple fields in the documents.


## UE Resources
- [$ variable](https://docs.mongodb.com/manual/reference/operator/projection/positional/)
- [design patterns](http://thetechnick.blogspot.com/2016/06/mongodb-design-patterns.html)
