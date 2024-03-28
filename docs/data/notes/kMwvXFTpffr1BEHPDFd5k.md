
### What is it
A data source generally sits on a server, and acts as the broker between the server and the data from the data layer. A data source offers generic methods that declaratively show what information the data source can provide. How the data source actually accesses that data is abstracted away from those who use the data source.
- you can think of a *data source* as the "client" on a server, since it's essentially the interface to the data layer.

### Why use it?
The benefit here is that if something changes on the data layer (such as the shape of the data), we only need to make adjustments in the data source. Without a data source, every component that consumes data from the database would need to change how it receives that data.

### Other value-adds
Apollo server has a data source class that can help with caching, [[deduplication|storage.deduping]]