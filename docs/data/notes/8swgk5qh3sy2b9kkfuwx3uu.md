
Caches are used in almost every computing layer: hardware, operating systems, web browsers, web applications, and more. 
- they are often found at the level nearest to the client (term used loosely to mean any component that makes requests), where they are implemented to return data quickly without taxing downstream levels.

## Cache invalidation
Cache Invalidation is the idea that if the data is modified in the data layer, it should be invalidated in the cache; if not, this can cause inconsistent application behavior.

Cache invalidation involves an action that has to be carried out by something other than the cache itself. Something (e.g., a client or a pub/sub system) needs to tell the cache that a mutation happened.

To solve this, there are three main schemes that are used:
1. *Write-through cache*: Under this scheme, data is written into the cache and the corresponding database simultaneously. The cached data allows for fast retrieval and, since the same data gets written in the permanent storage, we will have complete data consistency between the cache and the storage. Also, this scheme ensures that nothing will get lost in case of a crash, power failure, or other system disruptions.
    - Although write-through minimizes the risk of data loss, since every write operation must be done twice before returning success to the client, this scheme has the disadvantage of higher latency for write operations.

2. *Write-around cache*: This technique is similar to write-through cache, but data is written directly to permanent storage, bypassing the cache. This can reduce the cache being flooded with write operations that will not subsequently be re-read, but has the disadvantage that a read request for recently written data will create a “cache miss” and must be read from slower back-end storage and experience higher latency.

3. *Write-back (ie. write-behind) cache*: Under this scheme, data is written to cache alone, and completion is immediately confirmed to the client. The write to the permanent storage is done after specified intervals and/or under certain conditions (such as low traffic periods). This results in low-latency and high-throughput for write-intensive applications; 
- this performance benefit comes with the risk of data loss in case of a crash or other adverse event because the only copy of the written data is in the cache.
    - To mitigate this risk, write-back caches may employ mechanisms like write-ahead logging or transaction logs to recover data in case of failures.
- this cache pattern is an example of weak [[consistency|db.acid.consistency]]


### with ETags
[[ETags|protocol.http.etag]] identify a certain version of a resource.
- ex. An API can send back an ETag with a request for a list of 20 items. This ETag identifies this unique batch of items. If an identical request is made to the same endpoint and the ETag is also identical, then we know that the data is the same as well. We can use this ETag to make decisions on whether we want to fetch again or not. If however, our identical request shows a different ETag, then we take that to mean that the data from origin has somehow changed. We can use this information to determine whether or not we should invalidate data in a cache, or whether or not to render a long list of items on the page, etc.

### Cache eviction policies
Cache eviction means to free the memory of the old, unused data in the cache.

There are many cache eviction strategies, and what makes it difficult is that there is no one-size-fits-all approach. Agreeing to use one and not another can be a difficult endeavor.

Following are some of the most common cache eviction policies:
- Least Recently Used (LRU): Discards the least recently used items first (most commonly used).
- Most Recently Used (MRU): Discards, in contrast to LRU, the most recently used items first.
- Least Frequently Used (LFU): Counts how often an item is needed. Those that are used least often are discarded first.
- First In First Out (FIFO): The cache evicts the first block accessed first without any regard to how often or how many times it was accessed before.
- Last In First Out (LIFO): The cache evicts the block accessed most recently first without any regard to how often or how many times it was accessed before.
- Random Replacement (RR): Randomly selects a candidate item and discards it to make space when necessary.

### Why is cache invalidation considered difficult?
Success in a caching implementation involves seeing into the future, or multiple possible futures, which is hard in any non-deterministic situation.
- Sources of non-determinism include both your own workload as well as other workloads that may affect the cost of things you depend on (cache fill / eviction actions, for example).

The two main reasons why cache invalidation is difficult:
1. Space complexity issues
    - You have to guess when the data is not likely to be needed in memory; It can't be too short that the cache is useless, and too long that you'll get a [[memory leak|memory#memory-leak]].
    - Suppose we had infinite memory, then cache all the data; but we don't so we have to decide what to cache that is meaningful to have the cache implemented (is a ??K cache size enough for your use case? Should you add more?) - It's the balance with the resources available.

2. Time complexity issues
    - The underlying data might get changed by another process and then your process that uses the cache will be working with incorrect data
    - Suppose all your data was immutable, then cache all the data indefinitely. But this isn't always to case so you have to figure out what works for the given scenario (A person's mailing address doesn't change often, but their GPS position does).

Etags identify a certain version of a resource.
- ex. An API can send back an ETag with a request for a list of 20 items. This ETag identifies this unique batch of items. If an identical request is made to the same endpoint and the ETag is also identical, then we know that the data is the same as well. We can use this ETag to make decisions on whether we want to fetch again or not. If however, our identical request shows a different ETag, then we take that to mean that the data from origin has somehow changed. We can use this information to determine whether or not we should invalidate data in a cache, or whether or not to render a long list of items on the page, etc.

## UE Resources
- [Solving cache consistency (Facebook engineering article)](https://engineering.fb.com/2022/06/08/core-data/cache-invalidation)
- [Cache replacement policies (e.g. MRU, LRU)](https://en.wikipedia.org/wiki/Cache_replacement_policies)