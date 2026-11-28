
Redis transactions provide us with 2 guarantees:
1. All the commands in a transaction are serialized and executed sequentially. 
    - This means that a request sent by another client will never be served in the middle of the execution of the transaction.
2. The `EXEC` command triggers the execution of all the commands in the transaction, 
    - so if a client loses the connection to the server in the context of a transaction before calling the `EXEC` command, none of the operations are performed

A Redis Transaction is entered using the `MULTI` command
- At this point the user can issue multiple commands. Instead of executing these commands, Redis will queue them. They will all be executed once `EXEC` is called.
- Calling `DISCARD` will flush the transaction queue and will exit the transaction.