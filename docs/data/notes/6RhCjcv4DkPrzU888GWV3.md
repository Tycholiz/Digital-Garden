
Apollo caches the result of every graphql query in a normalized [[cache|general.arch.cache]].
- The cache normalizes query response objects before it saves them to its internal data store.
- though like the result of a graphql query, the object can be arbitrarily deep. However, references are used, so that the same object is never stored twice.

### Normalization
Normalization happens in the following steps:
1. The cache generates a CacheId for every identifiable object included in the response. 
	- The CacheId will be in format: `<__typename>:<id>` (ex. `Bucket:232`)
2. All the objects are stored by that generated ID in a flat lookup table.
3. Whenever an incoming object is stored with the same CacheId as an existing object, the fields of those objects are merged.
	- this means that the only time anything is overwritten is when the field names are the same. If the incoming object has different fieldnames than the existing one, they will be preserved 
	- ex. this has the further implication that if we had one query that returned a list of books (but only the title and author), then we subsequently clicked on a book and another query was made to get all the metadata data (description, page count, reviews etc.), that single book would not have been overwritten. The additional fields from the second query would have simply been merged.

The apollo cache takes the following shape:
```json
{
  "__typename": "Person",
  "id": "cGVvcGxlOjE=",
  "name": "Luke Skywalker",
  "homeworld": {
    "__ref": "Planet:cGxhbmV0czox"
  }
}
```

## Interacting directly with the Cache
We can read/write to the Apollo cache without interacting with our Graphql server.

We can do this in a few different ways:
- using regular Graphql queries to manage both remote and local data
	- `readQuery` / `writeQuery` / `updateQuery`
- using Graphql fragments to access the fields of a cached object without having to compose an entire query to reach that object.
	- `readFragment` / `writeFragment` / `updateFragment`
- modify the cache directly without using Graphql at all
	- `cache.modify`

### Manually updating the cache
When a single resource is updated with a mutation, Apollo handles caching for us (as long as we return the `id` and fields that were updated). 
- creating, deleting, or updating many at once will require us to manually update the cache.

![[apollo.client.cache.api]]

### Local-only fields
We can store and retrieve fields that only exist in the cache by using the `@client` decorator on our graphql query.
```gql
query ProductDetails($productId: ID!) {
  product(id: $productId) {
    name
    price
    isInCart @client # This is a local-only field
  }
}
```

* * *

### Type Policies
By defining type policies, we can determine how the cache interacts with specific types in the schema
- done by mapping a `__typename` to the whole `TypePolicy` object.
- in other words, the `typePolicies` object has `key`-`values` of `__typename`-`TypePolicy Object`

each field in a `typePolicies` object is a type's `__typename`

we can customize how a particular field within our Apollo cache is written to and read. For this, we have 2 methods: `merge` and `read`.
- with `read`, the cache calls that function whenever your client queries for the field. In the query response, the field is populated with the read function’s return value, instead of the field’s cached value.
	- Read is useful for manipulating values when they’re read from the cache, for example, things like formatting strings, dates, etc.
- with `merge`, the cache calls that function whenever the field is about to be written with an incoming value. When the write occurs, the field’s new value is set to the merge function’s return value, instead of the original incoming value.
	- Merge can take incoming data and manipulate it before merging it with existing data. Suppose you want to merge arrays or non-normalized objects.
- to define the policy for a single field, we need to first know which TypePolicy object the field corresponds to.

`FieldPolicy` lets us customize how individual fields in the Apollo Client cache are read and written.
