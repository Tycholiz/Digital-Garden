
## Overview
Postgres has a system of asynchronous messages and notifications, implemented by `listen` and `notify` keywords.
- This means that as soon as a connection is established with PostgreSQL, the server can send messages to the client even when the client is idle.
	- This method of communication is also carried out with the `COPY` command.

Commonly, the channel name is the same as the name of some table in the database, and the notify event essentially means, "I changed this table, take a look at it to see what's new"

## Notify
`notify` provides a simple interprocess communication (IPC) mechanism for a collection of processes accessing the same PostgreSQL database.

When `notify` is used to signal the occurrence of changes to a particular table, a useful programming technique is to put the `notify` in a rule that is triggered by table updates.
- In this way, notification happens automatically when the table is changed, and the application programmer cannot accidentally forget to do it.

In a transaction, `notify` events are not delivered until the surrounding transaction has been committed.

## Listen
When we execute `listen`, we are registering the current postgres session as a listener on the specified notification channel.
- when the specified channel is `notified` (either by the current session, or another one connected to the same db), all sessions subscribed to that channel will receive the message, and will in turn pass it on to the client whom it is connecting.

The payload passed to the client includes 3 things:
1. Notification channel name
2. Session server's PID
3. Payload string (`''` if unspecified)

A session's listen registrations are automatically cleared when the session ends.

The method a client application must use to detect notification events depends on which PostgreSQL API it uses (ie. libpq, libpgtcl)

## `pg_notify(channel_name text, payload text)`
We can also send a notification using the `pg_notify` function
- The function is much easier to use than the `notify` command if you need to work with non-constant channel names and payloads.

## Example
```psql
psql## listen my_notification_channel;
LISTEN

psql## notify my_notification_channel, 'foo';
NOTIFY
Asynchronous notification "my_notification_channel" with payload "foo"  ⏎
received from server process with PID 40430.
```

## UE Resources
[Dmitiri Fontaine on Listen-Notify](https://tapoueh.org/blog/2018/07/postgresql-listen-notify/)
