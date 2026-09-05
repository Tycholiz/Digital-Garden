
## Philosophy
declaratively define the connection between the component, and the data you want it to display. When the data changes, every component that is connected will automatically update.
watermelon is fast in part because it uses a declarative API. The declarative API means that all of the expensive computation is being done natively (Java or Swift). Since Javascript is quite slow compared to these 2 languages, this allows our computations to be done more efficiently.

## Tables
#### Columns
Columns have one of three types: string, number, or boolean
Fields of those types will default to '', 0, or false respectively

To allow fields to be null, mark the column as isOptional: true

#### withObservables
- This is the principal way that we connect WatermelonDB to our component
- let's us enhance a component by turning a non-reactive component to become reactive, meaning that UI will update in accordance with localdb changes
- we make our component reactive by feeding it an observable for the data we want to display

```js
withObservables(['post'], ({ post }) => ({
  post: post.observe(), // inject enhanced props into the component
  author: post.author.observeCount()
}))
```
- above:
	- `({ post })` are the input props for the component
	- The first argument: `['post']` is a list of props that trigger observation restart. So if a different post is passed, that new post will be observed
	- Rule of thumb: If you want to use a prop in the second arg function, pass its name in the first arg array
	- This is also the place that we should make relations
		- the relation is enabled by the `@children` decorator on the parent model

## Actions
Mutation (Create, Update) queries can be made from anywhere in the app, but the preferred way is to execute them through actions
- An action is a function that can modify the database

## Migrations
Each migration must migrate to a version one above the previous migration
- of course, each migration simply builds on the previous ones, meaning that when we want to make changes, we need to add the new changes as an item in the `migrations` array (to the front) and mark it with the next integer `toVersion`.

Steps to making schema changes:
1. make the change in migrations.js
2. wait for the error in the simulator:
	- `Migrations can't be newer than schema. Schema is version 1 and migrations cover range from 1 to 2`
3. if the error is there, make the change in schema.js, updating the `schemaVersion` to the latest migration

#### Testing migrations work properly
1. *Migrations test*: Install the previous version of your app, then update to the version you're about to ship, and make sure it still works
2. *Fresh schema install test*: Remove the app, and then install the new version of the app, and make sure it works

## Q Module
- This module provides us to make SQL-like clauses to help construct our query
	- This is where we can use WHERE, JOIN (on), AND, OR, LIKE etc.
		- note: remember to escape `Q.like`
- JOINs are done through `Q.on`

## Observable
- `.observe()` will return an observable
- we can hook up observables to components
- Because WatermelonDB is fully observable, we can create a @lazy function that will observe a database value and give us updated results in real-time (ie. without having to query the database)
	- ex. imagine we have a blog site, and blog posts can have a "popular" banner if they have at least 10 comments. We can make a function on the model layer that will observe the number of comments and will reactively give us the correct flag for the boolean:
	```js
	class Post extends Model {
		@lazy isPopular = this.comments.observeCount().pipe(
			map$(comments => comments > 10),
		    distinctUntilChanged()
		)
	}
	```
	and then directly connect it to the component:
	```js
	const enhance = withObservables(['post'], ({ post }) => ({
		isPopular: post.isPopular,
	}))
	```
	- since this is reactive, a rise above/fall below the 10 comment threshold will cause the component to re-render.
	- Dissecting:
		- `this.comments.observeCount()` - take the Observable number of comments
		- `map$(comments => comments > 10)` - transform this into an Observable of boolean (popular or not)
		- `distinctUntilChanged()` - this is so that if the comment count changes, but the popularity doesn't (it's still below/above 10), components won't be unnecessarily re-rendered
		- `@lazy` - also for performance (we only define this Observable once, so we can re-use it for free)

## Sync
Any backend will work, as long as it complies with the following spec:
![6d0a0837a90af34681ce452b015d4b19.png](:/860953c1a363424aac94bdff9d490b90)
- `changes` is an object with a field for each model (table) that has changes. For each model, there are 3 fields: `created`, `updated`, `deleted`.
	- When the `changes` object is received from a Pull Request, it is the selection of changes that were made on the server since our last sync, that we need to now update locally.
	- When the `changes` object is sent with a Push Request, it is the selection of changes that we've made locally, that have not yet been sent to the remote database.

#### Pulling
When Watermelon runs `synchronize()`, `pullChanges` will get run, which will pass along with it information about the last time a pull was made (`lastPulledAt`). `pullChanges` will call an endpoint to the backend, passing along that `lastPulledAt` timestamp, and the server will confer with the backend DB, and send back all of the changes made since the last pull, along with the current timestamp. When the mobile app receives the response, it will then proceed to apply those changes to the local db.

#### Pushing
We send to the server a `change` object, containing everything that needs to be updated remotely, as well as a timestamp of the last time a pull was made (`lastPulledAt`). When the server receives the request, it will use `lastPulledAt` to check conflicts with the remote db. If there is no conflict, the server will then update the db with the provided changes.

#### Sync limitations
There are currently limitations of Sync, as outlined in this blog: [How to Build WatermelonDB Sync Backend in Elixir | Fahri NH](:/f615cdc32a5c4be4a768caee30774aa9)

#### How does it know when to re-render?
for individual records, just listen to changes, and if the record changes, re-render

for queries, like "tasks where a=b", listen to the collection of tasks, and when a record in that collection changes, check if the record matches the query. If it does: if record was on the rendered list, and was deleted — remove from rendered list. if it wasn't on the rendered list, and now matches — add to rendered list.

for multi-table queries like "tasks that belong to projects where a =b", listen to all relevant collections, and if there's a change in any of them, re-query the database. There's ways to make it more efficient, but need to measure if the perf benefit is worth it

## Solutions to:
- [prop drilling](https://nozbe.github.io/WatermelonDB/Components.html#database-provider)

## UE Resources
[Logrocket Tutorial](https://blog.logrocket.com/offline-app-react-native-watermelondb/)
[how sync works](https://fahri.id/posts/how-to-build-watermelondb-sync-backend-in-elixir/)
[conf](https://www.youtube.com/watch?v=uFvHURTRLxQ)
[Pull/Push changes synchronization controller API](https://github.com/FLVieira/sync-api/blob/master/src/app/controllers/SynchronizationController.js)
[#2 Pull/Push changes synchronization controller API](https://github.com/rodrigosuman/rn-offline-first-app/tree/main/backend/src/services/sync)
