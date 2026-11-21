
Everything in the Studio starts with the document. A document is what you create and edit in the studio—all the other types you may define live inside the documents. In the default studio configuration, the document-types are the ones that will be listed in the content-column.

A reference in Sanity is a link from one document to another

Standard references are “hard” meaning when a document references another document, the target document must exist, and is actually prevented from being deleted until the reference is removed.
- There are also weak-references that do not "hold on to" the target. You make them by adding a _weak-key to the reference object like this: `{_ref: "<document-id>", _weak: true}`

an object containing a `_ref` key appearing anywhere in the document becomes a hard reference, and must be followed with the dereferencing operator `->` in order to access its values:
