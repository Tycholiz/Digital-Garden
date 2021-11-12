
# Connections
- a connection is a collection of objects with metadata such as `edges`, `pageInfo`...
	- It is effectively the edges, and the metadata associated with their environment.
- `pageInfo` will contain `hasNextPage`, `hasPreviousPage`, `startCursor`, `endCursor`
	- `hasNextPage` will tell us if there are more edges available, or if we’ve reached the end of this connection.

- A connection is a paginated field on an object — for example, the friends field on a user or the comments field on a blog post.
- very similar to cursor-based pagination

Connections are made up of edges, but a connection is more than just that, since it also contains metadata about the group of edges, such as `pageInfo`, which gives us pagination info (ex. what the most recent cursor was, if there is another page or not, etc)
![](/assets/images/2021-03-09-21-57-34.png)
In the above image, the connections would be all of the grey lines together as one. 

We might define our `User` type like this:
```
type User {
  id: ID!
  name: String
  friendsConnection(
    first: Int,
    after: String,
    last: Int,
    before: String
  ): UserFriendsConnection
}
```

This might lead to a query like so:
```
{
  user(id: "ZW5jaG9kZSBIZWxsb1dvcmxk") {
    id
    name
    friendsConnection(first: 3) {
      edges {
        cursor
        node {
          id
          name
        }
      }
    }
  }
}
```
A connection is a way to get all of the nodes that are connected to another node in a specific way
- In this case we want to get all of the nodes connected to our users that are friends. Another connection might be between a user node to all of the posts that they liked.

A connection is by nature an abstract concept, and it is difficult to think about. An edge makes sense, because we can think of a user having a friendship with another user, or we think of a user authoring a post.

A connection type may look like so:
```
type UserFriendsConnection {
  pageInfo: PageInfo!
  edges: [UserFriendsEdge]
}
```
