
Every GraphQL service has a query type and may or may not have a mutation type

### Understanding how a query works
```gql
query {
  hero {
    name
    appearsIn
  }
}
```

1. We query the special root Query type, and get back an object
2. We select the hero field on that object
3. For the object returned by hero, we select the name and appearsIn fields
