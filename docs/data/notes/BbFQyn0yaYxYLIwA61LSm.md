
# Fields
Every field on a GraphQL object type can have zero or more arguments:
```
type Starship {
  id: ID!
  name: String!
  length(unit: LengthUnit = METER): Float
}
```
- REST endpoints are similar to GraphQL fields, as they are entry points into the data that call functions on the server.
- sometimes you can have an entity that is both a field and a type. When I query for one nugget, I am querying on a field `nugget`, and getting back a Nugget type.
	- in this case, `nugget` is known as the root field
- Each field on each type is backed by a *resolver*
	- When a field is executed, the corresponding resolver is called to produce the next value. If a field produces a scalar value like a string or number, then the execution completes. However if a field produces an object value then the query will contain another selection of fields which apply to that object. 
		- This continues until scalar values are reached. GraphQL queries always end at scalar values.

Fields are conceptually functions which return values, and occasionally accept arguments which alter their behavior.
- These arguments often map directly to function arguments within a GraphQL server’s implementation.
- you can think of each field as a function of the previous type which returns the next type
	- therefore, fields can exist at different levels. Imagine we had a `User` type, and that `user` had an `id` and `name`. We would be looking at 3 fields in total here, since `user` is just a field defined on the `Query` type. 

Since fields are conceptually just functions, that means we may be able to pass arguments to them. 
- Arguments can also be passed into scalar values, which would allow the server to do data transformations before sending it to the client (as opposed to the client having to handle that). All we need to do is define an enumeration type:
```
type Person {
	name: String
	picture(size: Int): Url
}

{
  person(id: "1000") {
    name
    picture(size: 600)
  }
}
```

## Alias
By default, the response of our query is an object with a key matching the field name(s) we queried

Sometimes we want to request data about 2 identical objects. (ex. querying the same field twice, but with different arguments). In cases like this, we need to use aliases, which changes the key of the object we get as our query result.

