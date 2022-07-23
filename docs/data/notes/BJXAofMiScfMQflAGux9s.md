
### Keys
Try to stick with a schema. For instance `object-type:id` is a good idea, as in "user:1000". Dots or dashes are often used for multi-word fields, as in `comment:1234:reply.to` or `comment:1234:reply-to`.

### Hash
`field` and `value` form a pair. Both are strings
- If this were Javascript, 
    - the object name would be the Redis `key`, 
    - the object key would be the Redis `field`, 
    - the object value would be the Redis `value`

```
user:1000: {
    "username": "antirez",
    "birthyear": 1970
}
```

Methods:
- `HGET <key> <field>` / `HSET <key> <field> <value>` - get/set a field+value

### Lists
Redis lists are implemented via Linked Lists.
- LinkedLists were used (as opposed to arrays) because for a database system it is crucial to be able to add elements to a very long list in a very fast way.
- When fast access to the middle of a large collection of elements is important, Sorted Sets should be used

Common use-cases for Lists
- Remember the latest updates posted by users into a social network.
    - ex. Twitter takes the latest tweets posted by users into Redis lists.
    - imagine your home page shows the latest photos published in a photo sharing social network and you want to speedup access.
        - Every time a user posts a new photo, we add its ID into a list with `LPUSH`.
        - When users visit the home page, we use `LRANGE 0 9` in order to get the latest 10 posted items.
- Communication between processes, using a consumer-producer pattern where the producer pushes items into a list, and a consumer (usually a worker) consumes those items and executed actions. Redis has special list commands to make this use case both more reliable and efficient.

Methods:
- `LPUSH` / `RPUSH` - add element to the left/right
- `LRANGE <listname> <fromindex> <toindex>` - extract ranges of elements from lists

### Sets
unordered collections of strings.

Sets are good for expressing relations between objects. For instance we can easily use sets in order to implement tags.
- A simple way to model this problem is to have a set for every object we want to tag. The set contains the IDs of the tags associated with the object.

Methods:
`SADD` - add element to set

### Sorted Sets
similar to a mix between a Set and a Hash
- Like sets, sorted sets are composed of unique, non-repeating string elements, so in some sense a sorted set is a set as well.
- every element in a sorted set is associated with a floating point value, called the score (this is why the type is also similar to a hash, since every element is mapped to a value).

Methods:
`ZADD` - add element to set

# E Resources
[Redis Documentation](https://redis.io/topics/data-types-intro)
