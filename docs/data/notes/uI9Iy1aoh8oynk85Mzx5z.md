
# Commit Object
- stores metadata about the commit, like who authored it, when it was made, what the previous commit is etc.
- a commit points to a single tree, marking it as what the project looked like a certain point in time
	- Therefore, the commit object is what gives a project its sense of history in Git
- While the tree and blob objects are the content of the Git, the commit objects allow us to use the data in a user-friendly way. Technically, Git could be used without commit objects. However, we would have to remember every SHA to recall the snapshots. Also, we wouldn't have important information like who saved the snapshots, when they were saved, and who saved them. These benefits are what commit objects bring to us.
- When we run `git commit`, a commit object is created, and the parent commit is specified as the commit that the HEAD file points to (recall that a branch is just a pointer to a commit)
