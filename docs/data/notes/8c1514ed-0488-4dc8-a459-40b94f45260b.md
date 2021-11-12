
# Edge
An edge describes the relationship between a parent object and the target node. In our graph above, we have a central node (the parent) that points to other objects. The relationship (edge) is that of friendship. The entity on the other end of the line is the node.
- An edge has metadata about one object in the paginated list, and includes a cursor to allow pagination starting from that object.
- Each edge has a node and a cursor 

An edge type may look like so:
```
type UserFriendsEdge {
  cursor: String!
  node: User
}
```
Edges are not just valuable for pagination purposes.
In graph theory an edge can have properties of its own, which act effectively as metadata.
It has become common to think of the `edges` field as boilerplate, and simply a container for the cursor, but it actually can be quite powerful. When we consider that an edge is a field that represents the relationship between 2 nodes, we can start to realize there is a lot of potentially appropriate fields to describe that connection.
- ex. In the friend graph above, we can keep metadata about the friendship, such as when it started.
- ex. Relevancy score when searching (e.g the relevancy of this result to the input search query is 0.7).
- ex. Distance when searching/sorting by distance from a certain location.

In the above cases, the field doesn't belong on the node because its value changes depending on the connection parameters. Remember, you should be able to get to the same record (e.g. node) via multiple routes within a single query and the values for its fields should be the same, so contextual data has to go somewhere else -- the edge types.

## Pagination
Edges enable us to perform cursor-based pagination. Since the cursor is (usually) just an `id`, we can use the `after` argument on a list, passing that `id`. This allows us to specify the starting point that data should be retrieved from
