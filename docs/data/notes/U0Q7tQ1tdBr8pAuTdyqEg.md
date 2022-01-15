
Vanilla Postgres allows us to define workers which watch a new events channel, and attempt to claim a new job whenever one is pushed to the channel.
- Postgres also lets other services watch the status of the events with no added complexity.
