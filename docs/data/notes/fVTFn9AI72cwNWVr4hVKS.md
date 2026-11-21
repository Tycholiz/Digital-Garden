
An aggregation allows us to process data records and return computed results.
- group values from multiple documents and give the ability to perform operations on the grouped data, returning a single result

You can use aggregation operations to:
- Group values from multiple documents together.
- Perform operations on the grouped data to return a single result.
- Analyze data changes over time.

Can be done in 3 ways:
[more info](https://docs.mongodb.com/manual/aggregation/#aggregation-map-reduce)
- Aggregation pipeline
- Map-reduce function
- single purpose aggregation methods

## Aggregation Pipeline
Aggregation pipelines are the preferred way to perform aggregations in Mongo.

An aggregation pipeline consists of one or more stages that process documents
- Each stage performs an operation on the input documents. 
  - ex. a stage can filter documents, group documents, and calculate values.
- The documents that are output from a stage are passed to the next stage.
- An aggregation pipeline can return results for groups of documents. 
  - ex. return the total, average, maximum, and minimum values.

### `$facet`
A facet processes multiple aggregation pipelines within a single stage (ie. the `$facet` stage) on the same set of input documents.
- input documents are passed to the `$facet` stage only once.
- `$facet` enables various aggregations on the same set of input documents, without needing to retrieve the input documents multiple times.

Similar to the `group by` function of a SQL database

ex. in Amazon when you make a search, the sidebar will show relevant things to group by, such as brand, price range etc. If their database was built in Mongo, it would be accomplished with facets.

The `$facet` stage has the following form:
```js
{ $facet:
   {
      <outputField1>: [ <stage1>, <stage2>, ... ],
      <outputField2>: [ <stage1>, <stage2>, ... ],
      ...
   }
}
```

- [docs](https://www.mongodb.com/docs/manual/reference/operator/aggregation/facet/)