
Open redis-cli shell - `redis-cli -h <host>`

- Get all keys - `keys *`
  - consider KEYS as a command chiefly for development/debugging purposes. It is not performant. If you're looking for a way to find keys in a subset of your keyspace, consider using SCAN or sets.
- Get all key's matching prefix pattern - `keys <level1>:<level2>:*` (e.g. `keys books:author:*`)
- Get key's value - `get <key>`
- Set new key to hold string value - `set <key> <value>`
- Sets field in the hash stored at key to value - `set <key> <field> <value>`
- Get the type of a particular key - `type <key>`
- Delete key - `del <key>`
- check how much TTL - `ttl <key>`
- force expire a key - `expire <key> 1`
