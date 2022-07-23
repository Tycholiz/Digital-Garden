
Mac apps publish a dictionary of addressable objects and operations. Applescript leverages this dictionary to be able to communicate with those programs.
- These dictionaries can be viewed with the Script Editor program, and then File > Open Dictionary (`cmd+shift+o`)
- At its core, this scripting dictionary is a `.sdef` file that is stored in the app bundle.

Every scriptable app implements its own scripting features and exposes its own unique terminology through a scripting dictionary

### Types of Terminology
Suite - A suite is a grouping of related commands and classes
- includes terminology supported by most scriptable apps, such as an `open` command, a `quit` command, and an `application` class.

Command - A command is an instruction that can be sent to an app or object in order to initiate some action.
- ex. `delete`, `make`, `print`

Class - A class is an object within an app, or an app itself
- ex. Mail has an application class, a message class, and a signature class, among others

Property - A property is an attribute of a class. 
- ex. the message class in Mail has many properties, including `date received`, `read status`, and `subject`.

### Concepts
#### Inheritance
different classes often implement the same properties. 
- ex. in Finder, the file and folder classes both have creation date, modification date, and name properties
- Rather than defining these same properties multiple times throughout the scripting dictionary, Finder implements a generic `item` class
    - any properties of the `item` class also apply to the `file` and `folder` classes

#### Containment
Classes of a scriptable app reside within a certain containment hierarchy. The application is at the top level, with other classes nested beneath.
- ex. Finder contains `disks`, `folders`, `files`, and other objects.
- ex. Mail contains `accounts`, which can contain `mailboxes`, which can contain other `mailboxes` and `messages`.

