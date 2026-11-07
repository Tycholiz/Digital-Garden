
A commit object stores metadata about the commit, like who authored it, when it was made, what the previous commit is etc.
- while a [[tree|git.plumbing.objects.tree]] represents a particular directory state of a working tree, a commit represents that state in "time", and explains how to get there.

- a commit points to a single tree, marking it as what the project looked like a certain point in time
	- Therefore, the commit object is what gives a project its sense of history in Git
- While the tree and blob objects are the content of the Git, the commit objects allow us to use the data in a user-friendly way. Technically, Git could be used without commit objects. However, we would have to remember every SHA to recall the snapshots. Also, we wouldn't have important information like who saved the snapshots, when they were saved, and who saved them. These benefits are what commit objects bring to us.
- When we run `git commit`, a commit object is created, and the parent commit is specified as the commit that the HEAD file points to (recall that a branch is just a pointer to a commit)

### Boundary Commit
A boundary commit is the commit that limits a revision range but does not belong to that range. For example the revision range `HEAD~3..HEAD` consists of 3 commits (`HEAD~2`, `HEAD~1`, and `HEAD`), and the commit `HEAD~3` serves as a boundary commit for it.
- This is related to the concept of inclusive/exclusive ranges. In this comparison, the boundary commit would be an exclusive boundary.

More formally, git processes a revision range by starting at the specified commit and getting at other commits through the parent links. It stops at commits that don't meet the selection criteria (and therefore should be excluded) - those are the boundary commits.
[Source](https://stackoverflow.com/questions/42437590/what-is-a-git-boundary-commit)