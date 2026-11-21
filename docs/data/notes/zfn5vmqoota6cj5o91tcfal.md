
Instead of wrapping our component with `<ApolloProvider />` (as we normally do), we wrap it with `<MockedProvider />`, which gives us benefits:
- mocks all network calls, allowing us to test in isolation and get consistent results with each query (`mocks` prop)

### `mocks`
the `mocks` array takes objects with specific requests and their associated results. 

```ts
const mocks = [
  {
    request: {
      query: GET_DOG_QUERY,
      variables: {
        name: 'Buck',
      },
    },
    result: {
      data: {
        dog: { id: '1', name: 'Buck', breed: 'bulldog' },
      },
    }
  },
];
```

When the provider receives a `GET_DOG_QUERY` with matching variables, it returns the corresponding object from the result key.