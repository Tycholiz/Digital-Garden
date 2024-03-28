
`useMutation` has an `update` function, whose purpose is to modify your cached data to match the modifications that a mutation makes to your back-end data
- this method allows us to interact with the cache as if we were interacting with a graphql API. 
	- ex. we can make general queries, as well as use fragments to help
- we can interact directly with the cache with `readQuery, writeQuery, readFragment, writeFragment`, which are methods on the `ApolloClient` class
- we can get the cached version of an entity with `cache.identify`
	- an input `{ id: 1, title: '', mediaItems: [{...}] }` gives us `Nugget:1`

## Apollo `cache` object
- Here, queries mirror what our gql queries would look like to target the same data from a Graphql server. Therefore, it must be a complete and valid query.
- fragments on the other hand provide more *random access* (as opposed to *sequential access*) to our cached data.
	- therefore, we need to provide the cacheId as an argument to the fragment methods.

### `readQuery`
- like a regular graphql query, only it is performed on the cache, rather than the GraphQL API

### `writeQuery`
- uses the same shape as `readQuery`, but requires us to include a `data` option with the modifications we wish to make
- the shape of our query is not enforced, and we can write fields that aren't present in our Graphql schema.
- [docs](https://www.apollographql.com/docs/react/caching/cache-interaction/#writequery)

### `readFragment`
If the object in the cache is missing fields that exist on the fragment, then we will be returned `null`.

### `writeFragment`
Just like `readFragment`, but like `writeQuery`, it requires a `data` object with the fields that we are writing.

### `updateQuery` / `updateFragment`
Allows us to read and write cached data in a single method call.
- ApolloClient 3.5 and later
- [docs](https://www.apollographql.com/docs/react/caching/cache-interaction/#using-updatequery-and-updatefragment)

### `cache.modify` 
- [docs](https://www.apollographql.com/docs/react/caching/cache-interaction/#using-cachemodify)

`modify` is a method we can execute on our cache that lets us modify individual fields directly 
- differs from `writeQuery` and `writeFragment` in that it will circumvent any `merge` function we have defined, meaning that fields are always overwritten with exactly the values you specify.
- cannot write fields that do not already exist on the object in the cache.

### `cache.identify`
Returns the cacheId for the specified object