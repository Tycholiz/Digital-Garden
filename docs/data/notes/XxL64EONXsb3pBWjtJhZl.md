
Apple events are a high-level message-based form of Interprocess Communication (IPC), used to communicate between local or remote application processes

An Apple Event contains:
1. Attributes describing how the event should be handled
2. optional parameters to the event handler that receives the event

Example
- When we drag-n-drop a file onto the TextEdit.app icon in Finder, what happens is that Finder commands TextEdit to open that file by sending it an `odoc` (open doc) event with a list of file identifiers as its parameter
![](/assets/images/2021-05-07-09-48-28.png)

With proper bindings, Apple events can be created and sent with programming languages
- ex. from our client application, we might call `iTunes().play()`, causing a hook/Play event to be sent from the client application to iTunes, instructing it to start playing. Applications may respond to an incoming Apple event by sending a reply event back to the client application

## Apple Event Object Model (AEOM)
The AEOM is a view-controller layer that provides a user-friendly representation of the application's internal data, allowing clients to identify and manipulate parts of that structure via Apple events
- An incoming Apple event representing a particular command (get, set, move, etc.) is unpacked, and any object specifiers in its parameter list are evaluated against the application's AEOM to identify the user-level object(s) upon which the command should act
- The command is then applied these objects, with the AEOM translating this into operations upon the application's implementation-level objects
    - These implementation-level objects are mostly user-data objects in the application's Model layer

![](/assets/images/2021-05-07-09-57-56.png)

- The AEOM represents user data as a tree-shaped object graph, whose nodes are connected via 1:1 and/or 1:many relationships.
- AEOM objects are identified by high-level queries (comparable to XPath or CSS selectors), not low-level chained method calls.
- While the Apple Event Object Model is sometimes described by third-parties as being similar to DOM, this is inaccurate as AEOM operates at a much higher level of abstraction than DOM.

The AEOM is a tree-like structure made up of objects. These objects may contain descriptive attributes such as class, name, id, size, or bounds; for example:

```
finder.version
itunes.playerState
textedit.frontmost
finder.home
textedit.documents
itunes.playlists
```

Unlike other object models such as DOM, objects within the AEOM are associated with one another by relationships rather than simple physical containment
Relationships between objects may be one-to-one:
```
finder.home
itunes.currentTrack
```
or one-to-many:
```
finder.folders
itunes.playlists
```

to show how how this tree-like structure is not related to containment (as the DOM is), consider that the following object specifiers all identify the same objects (files on the user's desktop):
- the first specifier describes the files' location by physical containment; the other two use other relationships provided by the application as convenient shortcuts

```
finder.disks["Macintosh HD"].folders["Users"].folders["kyletycholiz"].folders["Desktop"].files

finder.desktop.files

finder.files
```

Here is the AEOM for a simple hypothetical text editor:
![](/assets/images/2021-05-07-10-08-32.png)
