
GraphQL suffers from the N+1 problem

- The number of queries grows exponentially with the depth of the query
- N1 problem occurs when you have to retrieve the same information multiple times. In graphql, the resolver for that particular field would have to be hit every single time, instead of the system understanding that it's the same result, and being more efficient about it.
  - ex. We have a query shaped as below. imagine that the article has 5 `comments` and all are written by the same person. In Graphql, that would be 5 times that the resolver got called, instead of once.

```gql
query {
  user(id: "abc") {
    name
    article(title: "GraphQL is great") {
      comments {
        text
        writtenBy {
          name
        }
      }
    }
  }
}
```

- N1 problem can be solved by DataLoader library
- [possibly redundant info](https://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem-in-orm-object-relational-mapping)
- [using a batchloader (like joinMonster) to solve N+1](http://www.petecorey.com/blog/2017/08/14/batching-graphql-queries-with-dataloader/?from=east5th.co)
