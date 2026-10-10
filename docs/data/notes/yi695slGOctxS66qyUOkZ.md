
The lineage of an object is irrelevant. What matters about an object is what it can do, not what it is descended from.

JavaScript object keys are always coerced to a string, so obj[0] is always the same as obj["0"].

As of ES6, objects have a predictable order, determined in one of 2 ways:
1. If *keys are numbers*, The key-value pair being inserted will obey numerical order
	- ex. if we have existing keys `4`, `8`, and `33`, then inserting a key-value pair with the key as `1` will put it in first position of the object
2. If *keys are strings*, The key-value pair being inserted will be appended to the end of the object

### Polymorphism with objects
Imagine we had the idea of a session in our code. Instead of being one type however, we want it to either evaluate to an object, or the id of the object. Therefore, either object or string. This enables us to achieve polymorphism in functions, since we can take in a `session` argument, and depending on its type, we can act accordingly.
```js
// code from graphile/starter
export const becomeUser = async (
  client: PoolClient,
  userOrUserId: User | string | null,
) => {
  await becomeRoot(client)
  const session = userOrUserId
    ? await createSession(
        client,
        typeof userOrUserId === 'object' ? userOrUserId.id : userOrUserId,
      )
    : null
  await client.query(
    `select set_config('role', $1::text, true), set_config('jwt.claims.session_id', $2::text, true)`,
    [process.env.DATABASE_VISITOR, session ? session.uuid : ''],
  )
}
```
