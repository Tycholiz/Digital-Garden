
### Approaches to common problems arising because of a distributed system
#### Key Generation System
Imagine we were making a URL shortener like TinyURL. As a database solution, we opt for a NoSQL approach key-value store, where each key is the shortform URL, and the value is the longform URL.

##### Naive approach
Each time a write request is made (ie. user creates a new shortform URL), the application server generates a new random 6 character string, and attempts to insert it into the DB. If that key exists already, then it tries again, until there is no failure. This is naive because it involves a lot of back and forth between application server and database, and results in an unpredictable time complexity.

##### Smarter approach
Have a standalone Key Generation Service (KGS), which generates the random 6 character strings in advance and stores them in a separate database (the Key-DB). Whenever a user wants to generate a new TinyURL, we take a key from the Key-DB and use it. This takes out the worry of collision and duplications of keys.

The purpose of the 2 tables is to handle read concurrency issues. The problem we are trying to solve is to not give the same key to two servers (as we don’t want two URLs to have the same short key)

In our Key-DB, we can have 2 tables to store the keys, for:
1. `unused_keys`
2. `used_keys`

The reason we have 2 tables is to solve read concurrency issues. We are trying to avoid a situation where we give the same key to two application servers. Since there is only 1 KGS in the system at a time, there are inherently no write concurrency issues.

The KGS can always keep some unused keys in memory, so that whenever a server needs them, it can provide them quickly. As soon as the KGS loads some keys in memory, it moves them from `unused_keys` to `used_keys`, so that we can ensure each server gets unique keys. It's true that if the KGS goes offline, all those keys will be lost (and will still exist in the `used_keys` table). This is ok, since there are such a large amount of keys anyway.

Since the KGS would be a single point of failure, we can have a standby replica, and whenever the primary server dies it can take over to generate and provide keys.

![](/assets/images/2021-10-13-11-24-19.png)
