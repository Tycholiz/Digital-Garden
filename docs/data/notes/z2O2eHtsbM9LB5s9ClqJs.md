
Simply a way to mark specific commits as special in some way (ex. tag a specific commit as a specific release, or something along those lines)
- tags and branches are similar, in that they both point to a specific commit (ie. they are pointers to snapshots). Theoretically, branches could perform the role of tags. We could just keep around a branch called *Release 2.0*. Instead, Git allows us to separate concerns.
    - A branch is a moveable pointer to a snapshot, while tags point at a single snapshot and never move. Also, tags are actually stored as objects, while branches are not.
- if we are doing something a little risqué (like undoing a rebase), it's good to make a backup `git tag BACKUP`. Then if we ever need to go back to it, run `git reset --hard BACKUP`

### Annotated vs. Lightweight Tags
There are 2 types of tag, lightweight and annotated
- *lightweight* - a reference to a commit that never moves
	- this type of tag does not create a tag object
- *annotated* - when we create an annotated tag, Git creates a tag object and then writes a reference point to it (rather than directly to the commit).
	- we usually want this one.

### Inner
- The fourth (and less integral) object functions similar to a commit object, in that it contains a tagger, a message, a date, and a pointer.
	- The main difference is that a tag normally points to a commit rather than a tree. In this sense it is similar to a branch reference, but it never moves (ie. it always points to the same commit).
