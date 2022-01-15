
# Cache
- Apollo caches the result of every graphql query in a normalized cache
- when a single resource is updated with a mutation, Apollo handles caching for us (as long as we return the `id` and fields that were updated). When we create or delete, or if we update many at once, we will have to manually take care of caching.
	- `useMutation` has an `update` function, whose purpose is to modify your cached data to match the modifications that a mutation makes to your back-end data
- we can interact directly with the cache with `readQuery, writeQuery, readFragment, writeFragment`, accessible from `ApolloClient` class
- we can get the cached version of an entity with `cache.identify`
	- an input `{ id: 1, title: '', mediaItems: [{...}] }` gives us `Nugget:1`

readQuery
- like a regular graphql query, only it is performed on the cache, rather than the GraphQL API

writeQuery
- we can use this function to update the apollo cache 

We can search only data that is in the cache (ie no API call made) by using the `@client` decorator

## Normalization
- The `InMemoryCache` normalizes query response objects before it saves them to its internal data store.
- normalization happens in the following steps:
	1. The cache generates a unique ID for every identifiable object included in the response. 
		- ID will be in format: `<__typename>:<id>` (ex. `Bucket:232`)
	2. All the objects are stored by that generated ID in a flat lookup table.
	3. Whenever an incoming object is stored with the same ID as an existing object, the fields of those objects are merged.
		- this means that the only time anything is overwritten is when the field names are the same. If the incoming object has different fieldnames than the existing one, they will be preserved 
- The apollo cache is an object where each key is the ID (`typename:id`), and the value is an object of the corresponding record from the database:
```
data: {
	'Bucket:1': {
		id: 1,
		title: 'Psychology',
		__typename: 'Bucket'
	}
}
```

### Updating the cache
- we can pass an `update` method to `useMutation`
	- this method allows us to interact with the cache as if we were interacting with a graphql API. 
		- ex. we can make general queries, as well as use fragments to help
- `cache.modify` is a method we can execute on our cache that lets us modify individual fields directly 
	- differs from `writeQuery` and `writeFragment` in that it will circumvent any `merge` function we have defined, meaning that fields are always overwritten with exactly the values you specify.

### Type Policies
- By defining type policies, we can determine how the cache interacts with specific types in the schema
	- done by mapping a `__typename` to the whole `TypePolicy` object.
	- in other words, the `typePolicies` object has `key`-`values` of `__typename`-`TypePolicy Object`
- each field in a `typePolicies` object is a type's `__typename`
- we can customize how a particular field within our Apollo cache is written to and read. For this, we have 2 methods: `merge` and `read`.
	- with `read`, the cache calls that function whenever your client queries for the field. In the query response, the field is populated with the read function’s return value, instead of the field’s cached value.
		- Read is useful for manipulating values when they’re read from the cache, for example, things like formatting strings, dates, etc.
	- with `merge`, the cache calls that function whenever the field is about to be written with an incoming value. When the write occurs, the field’s new value is set to the merge function’s return value, instead of the original incoming value.
		- Merge can take incoming data and manipulate it before merging it with existing data. Suppose you want to merge arrays or non-normalized objects.
	- to define the policy for a single field, we need to first know which TypePolicy object the field corresponds to.
- `FieldPolicy` lets us customize how individual fields in the Apollo Client cache are read and written.
