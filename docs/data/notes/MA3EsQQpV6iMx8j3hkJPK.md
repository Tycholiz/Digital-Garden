
## Capacity Estimation and Constraints
Consider how read-heavy the data in your application is. For instance, maybe 5x more people are uploading data than are downloading it (a 5:1 read:write ratio). Let's assume 1M writes per day, giving us 5M reads:

Can users upload as much as they want? For instance, if we were building Instagram, we would want to make efficient storage of photos a top priority.

### Leveraging User Expectations
Have a good understanding of user expectations while designing the application. For instance, in an Instagram-clone, it's not necessary that every user sees the latest content at any given time— we can tolerate some delay. This enables us to retract our focus from strict caching and replication implementations, and instead focus on things that are more important for our system.

How fast do writes need to be? How about reads?
- For Instagram, fast uploads would not be expected by users. Reads, on the other hand would be expected, since there is a high degree of swiping going on. For this, we'd want to have a more sophisticated caching system in place.
- We should keep in mind that web servers have a connection limit before designing our system. If we assume that a web server can have a maximum of 500 connections at any time, then it can’t have more than 500 concurrent uploads or reads. To handle this bottleneck, we can split reads and writes into separate services. We can have dedicated servers for reads and different servers for writes to ensure that uploads don’t hog the system. This has the added benefit of being able to scale each set of services independently.
- If users can upload a large amount of data, then efficient management of storage should also be a focus

How reliable is your app expected to be?
- if you are Instagram, your users expect 100% reliability. Therefore, you should store multiple copies of each file so that if one storage server dies, the photo can be retrieved from the other copy present on a different storage server.
    - If we want to have high availability of the system, we need to have multiple replicas of services running in the system so that even if a few services die down, the system remains available and running. Redundancy removes the single point of failure in the system.

### Redundancy
All potential bottlenecks and points of failure should be scrutinized. Think of how the system gets more complex as we implement [sharding|db.strategies.sharding]. Now we need a sharding rule to determine in which shard a row is stored. Suppose we implement round robin with `recordId % 10` to spread out the records evenly. Well, now we have a new problem: we cannot have the shards increment the IDs themselves, since we need the ID first before we can even determine which shard it belongs in. So to solve that, we need some sort of external key generating service. This service can generate IDs, store them in a table (to indicate that it is used) when a new item is created. Each shard can use this service, and we can have perfectly incremented IDs across all shards. However, one last issue arises. This key generation system is now a single point of failure, so we have to iterate further. We could spin up another such key generating service, and split the load between them by having one service give out even-numbered IDs and the other odd-numbered IDs. Finally, we can stick a load balancer in front of them to round-robin the load between them. In case of failure of the even-numbered service, the only consequence is that there will be more odd-numbered IDs; something we probably don't even care about.

### Estimation
From there, assume that each item is 10KB, so we need to store 10GB per day. If we want to store this data for an average of 10 years then we need storage capacity of 36TB.
- To keep some margin, we will assume a 70% capacity model (meaning we don’t want to use more than 70% of our total storage capacity at any point), which raises our storage needs to 51.4TB.

New items per second:
- 1M / (24 hours * 3600 seconds) ~= 12 pastes/sec
results in 
120KB of ingress per second.
- 12 * 10KB => 120 KB/s

Reads per second:
- 5M / (24 hours * 3600 seconds) ~= 58 reads/sec
results in
total data egress (sent to users) will be 0.6 MB/s.
- 58 * 10KB => 0.6 MB/s

This number of records totals 3.6 Billion in 10 years. Since 6 random base64 encoding ([A-Z, a-z, 0-9, ., -]) can be used to generate 68.7 billion unique strings, this should be sufficient for uniqueness.

We can cache some of the hot data points that are frequently accessed. Following the 80-20 rule, meaning 20% of hot data points generate 80% of traffic, we would like to cache these 20% data points.
- ex. if designing a paste-bin clone, cache the top 20% of most accessed pastes.

Since we have 5M read requests per day, to cache 20% of these requests, we would need:
- 0.2 * 5M * 10KB ~= 10 GB
