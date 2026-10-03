
### Query key
Query keys are used to manage caching
- therefore, the query key represents a set of data.
- therefore, if your query function depends on a variable, include it in your query key

Query keys are necessary so that the library can internally cache your data correctly and refetch automatically when a dependency to your query changes.

A query key is an array, and can be as simple as an array of strings, or can be a combination of strings and deeply nested objects
- as long as the query key is serializable, and unique to the query's data, you can use it.

The order of keys in an object does not matter, meaning the following are equivalent:
```ts
useQuery({ queryKey: ['todos', { status, page }], ... })
useQuery({ queryKey: ['todos', { page, status }], ...})
useQuery({ queryKey: ['todos', { page, status, other: undefined }], ... })
```

However, order of array elements does in fact matter