
With weak consistency, the data will be lost if the cache crashes before propagating the data to the database

### How does it work?
1. the client executes a write operation against the cache server
2. the cache writes the received data to the message queue
3. the cache sends an acknowledgment signal to the client
4. the event processor asynchronously writes the data to the database
