
Task Queues/Worker Queues are used as a mechanism to distribute work across threads or machines.
A task queue’s input is a unit of work called a task. Dedicated worker processes constantly monitor task queues for new work to perform.

Often, a task queue will use a message broker to mediate between clients and workers
- To initiate a task the client adds a message to the queue, the broker then delivers that message to a worker.

Task Queues like Celery require a configured message broker like Redis broker transports or RabbitMQ to be functional
- Realistically, you could roll your own using SQLite if you wanted to. You just need a service that will take care of the message queue for you.
- Graphile-worker has this functionality built-in, as it manages its own queue in your Postgres db.
