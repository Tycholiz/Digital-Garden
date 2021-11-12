
When git creates a merge commit it will also by default append a list of files that had conflicts to the commit message:
```
Conflicts:
    src/foo-service.c
    src/bar-client.c
```
This is a piece of useful information for us when poring over the history to find out what went wrong
- Even if your merge conflict was trivial, there is always a non-zero chance of introducing a bug when resolving a conflict, and seeing those lines in the merge commit message could be valuable information. They are basically a hint saying “Still confused? Maybe you should be extra careful when reviewing the changes in these files”.
