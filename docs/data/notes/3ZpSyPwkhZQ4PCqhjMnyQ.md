
Graphile defines a library `pg-pubsub`

The main high-level purpose of this library is provide our custom plugins with realtime data, which allows us to add subscription fields to our API.
- The library uses Postgres' `LISTEN`/`NOTIFY` to provide realtime features

This library also gives us the `@pgSubscription` directive that we can put on graphql fields


[[Pub-Sub|general.architecture.pub-sub]]
