
Winston is a logger library designed with support for multiple transports (containers for the logs)
- you can store errors in one transport, and normal logs in another. This is nice if you want to integrate other services. For instance what if we want our database to consume and store these errors, but don't care about storing other log levels?
