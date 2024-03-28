
## Syncing the same document
The three most common approaches to synchronization of a single document are *locking*, *event passing* and *three-way merges*.

### Locking
The simplest technique, whereby a shared document may only be edited by one user at a time.
- ex. when opening Microsoft Word on a networked drive, the first user to open it has global access, but subsequent users have read-only access.

A refinement on locking would be Subsection Locking, whereby we only lock the parts of the document that are being edited, but this still limits collaboration.

Locking is not a suitable approach when connectivity is unreliable, since the lock or unlock signals can get lost, leaving no owner.
- ex. imagine being in the middle of editing a document, and then passing through a tunnel. With luck, the user will still be editing when they come out of the other side of that tunnel, and will be in connectivity when they finally save and close the document. But what if they save and close while in the tunnel and out of connectivity? Whoever manages the lock will not be aware that the client as finished accessing the document, and it will assume that it should still be locked, thereby preventing access to any other clients.

### Event passing
Event passing involves capturing all user actions and replaying them on other nodes of the network.

Popular for implementing edit-based collaborative systems.

Any failure in event passing results in a fork
- due to the best-effort nature of the web, this is potentially commonplace. 
- ex. think of a SaaS like Jira, where you have many different users interacting with the same resources often (e.g. all members of engineering team editing the same sets of documentation). When there is a conflict, Jira just chooses one of the writes as the winner, and sends a "sorry, please try again" message to the losing client.
    - edit: not sure if this point belongs here. Not sure if this example actually demonstrates a fork. This point might belong elsewhere

### Three-way merge
1. The client sends the contents of the document to the server.
2. The server performs a three-way merge to extract the user's changes and merge them with changes from other users.
3. The server sends a new copy of the document to the client.

Three-way merges are not a good solution for real-time collaboration across a network with latency.

### Differential Synchronization
Differential synchronization is a symmetrical algorithm employing an unending cycle of background diffs and patch operations.
- https://neil.fraser.name/writing/sync/
- [Google whitepaper](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/35605.pdf)

* * *

## Synchronization Strategies
### Create some means to "diff" the the most recent 
Mimic the schemas of the client-side database and server-side database as much as possible

The key is for both the local database and remote database to track when each value was each of its fields were last changed. 
- To implement this, we can consider the fields we are interested in for a particular model (e.g. `title`, `media_items`). That is, we want to know when these values have changed. We can store this value by adding N more columns, which are timestamps of when the row was last updated (e.g. `title__last_updated`, `media_items__last_updated`). The client and remote databases must each update their own version of this column when the value changes, allowing the other databases to know who has fresher data.
    - ex. when remote database updates its value in response to a change added by the client
    - ex. when the client database receives a new value from the remote server.
    - ex. when the the user updates a document via the app's UI

### Log of Changes
Another approach is to collect a log of user interactions with the client database. We determine that there has been a change in state by the presence of a log indicating so. When this happens, we initiate a 2-way sync with the remote database.

When the remote database receives the changes with the 2-way sync pulse, it compares the values to the ones it has stored. With each version's timestamp, we award legitimacy to the value using *last-write wins* and make that the new canonical value for the field. If the remote database has changes that don't yet exist on the client database, then it will return those back to the client. Finally, the client proceeds to update those values.

Each client-side database should initiate a 2-way sync on 2 occasions:
- when there has been a change in state (e.g. creating a new document, deleting one, updating etc.)
- after 5 seconds of inactivity

### Push and Pull
The push function basically receives a set of data changes as a JSON object and writes them to the database, while the pull function receives the timestamp of the last sync and compiles a JSON object containing all changes the database has accumulated since this timestamps and returns it to the client.

