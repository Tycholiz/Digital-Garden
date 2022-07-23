
Technically, we could do string interpolation to make our queries dynamic. However, this wouldn't be a good idea because our client would be tasked with manipulating that string at runtime. Instead, Graphql provides us with first-class variables that allow us to factor-out the dynamic parts of a query (eg. userId, first: 10)

Variables must be either scalars, enums or input object types.
- Input Object Type: Passing in an object might be a more sensible solution than strings, if there is related data. For instance, imagine we are running a `createUser` mutation. Instead of passing `first_name`, `last_name`, as so on to the mutation, why not just define a type, and pass an object?:
```gql
mutation createUser($userInfo: UserInfoInput!) {
	createUser(userInfo: $userInfo)	{
		first_name
		last_last
	}
}
```
