
Caches take advantage of the locality of reference principle: recently requested data is likely to be requested again.

Caches are used in almost every computing layer: hardware, operating systems, web browsers, web applications, and more. 
- while they can exist at all levels in architecture, they are often found at the level nearest to the front end, where they are implemented to return data quickly without taxing downstream levels.

### Cache invalidation
Cache Invalidation is the idea that if the data is modified in the database, it should be invalidated in the cache; if not, this can cause inconsistent application behavior.

To solve this, there are three main schemes that are used:
1. *Write-through cache*: Under this scheme, data is written into the cache and the corresponding database simultaneously. The cached data allows for fast retrieval and, since the same data gets written in the permanent storage, we will have complete data consistency between the cache and the storage. Also, this scheme ensures that nothing will get lost in case of a crash, power failure, or other system disruptions.
    - Although write-through minimizes the risk of data loss, since every write operation must be done twice before returning success to the client, this scheme has the disadvantage of higher latency for write operations.

2. *Write-around cache*: This technique is similar to write-through cache, but data is written directly to permanent storage, bypassing the cache. This can reduce the cache being flooded with write operations that will not subsequently be re-read, but has the disadvantage that a read request for recently written data will create a “cache miss” and must be read from slower back-end storage and experience higher latency.

3. *Write-back cache*: Under this scheme, data is written to cache alone, and completion is immediately confirmed to the client. The write to the permanent storage is done after specified intervals or under certain conditions. This results in low-latency and high-throughput for write-intensive applications; however, this speed comes with the risk of data loss in case of a crash or other adverse event because the only copy of the written data is in the cache.

#### Cache eviction policies
Cache eviction means to free the memory of the old, unused data in the cache.

There are many cache eviction strategies, and what makes it difficult is that there is no one-size-fits-all approach. Agreeing to use one and not another can be a difficult endeavor.

Following are some of the most common cache eviction policies:
- Least Recently Used (LRU): Discards the least recently used items first (most commonly used).
- Most Recently Used (MRU): Discards, in contrast to LRU, the most recently used items first.
- Least Frequently Used (LFU): Counts how often an item is needed. Those that are used least often are discarded first.
- First In First Out (FIFO): The cache evicts the first block accessed first without any regard to how often or how many times it was accessed before.
- Last In First Out (LIFO): The cache evicts the block accessed most recently first without any regard to how often or how many times it was accessed before.
- Random Replacement (RR): Randomly selects a candidate item and discards it to make space when necessary.

#### Why is cache invalidation considered difficult?
Success in a caching implementation involves seeing into the future, or multiple possible futures, which is hard in any non-deterministic situation.
- Sources of non-determinism include both your own workload as well as other workloads that may affect the cost of things you depend on (cache fill / eviction actions, for example).

The two main reasons why cache invalidation is difficult:
1. Space complexity issues
    - You have to guess when the data is not likely to be needed in memory; It can't be too short that the cache is useless, and too long that you'll get a memory leak.
    - Suppose we had infinite memory, then cache all the data; but we don't so we have to decide what to cache that is meaningful to have the cache implemented (is a ??K cache size enough for your use case? Should you add more?) - It's the balance with the resources available.

2. Time complexity issues
    - The underlying data might get changed by another process and then your process that uses the cache will be working with incorrect data
    - Suppose all your data was immutable, then cache all the data indefinitely. But this isn't always to case so you have to figure out what works for the given scenario (A person's mailing address doesn't change often, but their GPS position does).


#### Shopping cart example
Let's say User1 adds 5 different things in their shopping cart. The inventory count of those items are cached in Redis or Memcache.

While the user is checking out, a Manual Inventory has been completed and gets loaded into the system. A manual inventory is the result of someone walking through a warehouse and counting items by hand. They discover that there are actually zero of a certain item that happens to be in User1's digital cart. The inventory person obviously doesn't know that.

User1 pays for the items that the system says they still have inventory for. Ideally, an error occurs as the purchasing system attempts to move a number of items from "in stock" to "sold", as there are actually none "in stock". This happens a lot when one item is next to another item and they have very similar packaging. Warehouse employees will accidentally ship the wrong item. If the cart checkout code was less defensive, it may just set a negative "in stock" value and the company would have a more serious problem of selling something they didn't own.

Hopefully, the person who wrote the Manual Inventory Entry code knows about the caching that the shopping cart does and sends them some sort of message that "inventory has been updated. Please invalidate these cache entries." But that requires that they know exactly what is being cached and how it's being cached; otherwise they may not give the right information to ensure that data is being invalidated. ie- "Please invalidate the following SKU's - " vs "Please invalidate all SKU's associated with Vendor XYZ".

* * *

## Parameter cache requirements
### Cache memory
How much cache memory should we have? We can start with 20% of daily traffic and, based on clients’ usage patterns, we can adjust how many cache servers we need.
- Alternatively, we can use a couple of smaller servers to store all the cached data.
- Since a modern-day server can have 256GB of memory, we can easily fit all the cache into one machine.

## When to cache
- We don't want to cache many keys that change continuously.
- We don't want to cache many keys that are requested very rarely.
- We want to cache keys that are requested often and change at a reasonable rate. For an example of key not changing at a reasonable rate, think at a global counter that is continuously INCRemented.
