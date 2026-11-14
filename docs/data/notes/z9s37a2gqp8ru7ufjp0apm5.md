
### Delete item from cache after useMutation `update` function
```ts
const handleDeleteBook = (bookId: string) => {
  deleteBookMutation({
    variables: {
      bookId,
    },
    update: (cache) => {
      const normalizedId = cache.identify({
        id: bookId,
        __typename: 'Book',
      });

      cache.evict({ id: normalizedId });
      cache.gc();
    },
  });
};
```

## Functions

### Delete from cache
```ts
export function deleteFromCache(
  thisCache: ApolloCache<DeleteBookMutation>,
  thisId: string,
): void {
  const normalizedId = thisCache.identify({
    id: thisId,
    __typename: 'Book',
  })
  thisCache.evict({ id: normalizedId })
  thisCache.gc()
}
```

### Update cache
```ts
export function updateFromCache(
  thisCache: ApolloCache<UpdateBookMutation>,
  book: Book,
  bookInput: BookInput,
): void {
  thisCache.writeFragment({
    id: `Book:${book.id}`,
    fragment: gql`
      fragment MyBook on Book {
        title
      }
    `,
    data: {
      title: bookInput.title ? bookInput.title : '',
    },
  })
}
```