
Distributed [[caches|general.arch.cache]] take advantage of the *locality of reference principle*: recently requested data is likely to be requested again.

#### Shopping cart example
Let's say User1 adds 5 different things in their shopping cart. The inventory count of those items are cached in Redis or Memcached.

While the user is checking out, a Manual Inventory has been completed and gets loaded into the system. A manual inventory is the result of someone walking through a warehouse and counting items by hand. They discover that there are actually zero of a certain item that happens to be in User1's digital cart. The inventory person obviously doesn't know that.

User1 pays for the items that the system says they still have inventory for. Ideally, an error occurs as the purchasing system attempts to move a number of items from "in stock" to "sold", as there are actually none "in stock". This happens a lot when one item is next to another item and they have very similar packaging. Warehouse employees will accidentally ship the wrong item. If the cart checkout code was less defensive, it may just set a negative "in stock" value and the company would have a more serious problem of selling something they didn't own.

Hopefully, the person who wrote the Manual Inventory Entry code knows about the caching that the shopping cart does and sends them some sort of message that "inventory has been updated. Please invalidate these cache entries." But that requires that they know exactly what is being cached and how it's being cached; otherwise they may not give the right information to ensure that data is being invalidated. ie- "Please invalidate the following SKU's - " vs "Please invalidate all SKU's associated with Vendor XYZ".

### When to cache
- We don't want to cache many keys that change continuously.
- We don't want to cache many keys that are requested very rarely.
- We want to cache keys that are requested often and change at a reasonable rate. For an example of key not changing at a reasonable rate, think at a global counter that is continuously incremented.

## Parameter cache requirements
### Cache memory
How much cache memory should we have? We can start with 20% of daily traffic and, based on clients’ usage patterns, we can adjust how many cache servers we need.
- Alternatively, we can use a couple of smaller servers to store all the cached data.
- Since a modern-day server can have 256GB of memory, we can easily fit all the cache into one machine.

## Examples
- [[redis]]
- Memcached
- Riak