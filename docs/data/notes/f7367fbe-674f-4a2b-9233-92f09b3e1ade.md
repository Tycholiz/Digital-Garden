
# Branch
- Simply a pointer to a specific commit
	- Therefore, creating a branch is nothing more than writing 40 characters (a SHA) to a file.
- The key difference is that it is moveable
- It’s important to realize that Git uses the tip of a branch to represent the entire branch. That is to say, a branch is actually a pointer to a single commit—not a container for a series of commits
	- this is why Git diagrams show commits pointing to other commits.
- If we make a new branch from master, that branch will point to the same commit as master. Once we commit, this new branch will then point to the new commit. Therefore, the branches will have diverged.
- When we create a new branch, Git knows which commit to use because of the HEAD file.

## Slashes in branch names
Imagine we had a branch named `stripe` already, and we wanted to make a new branch called `stripe/saved-methods`. This would cause an error, because as far as git knows, we have a file named `stripe`, and we are now trying to create a directory named `stripe`.
- Because they are just directories, deleting `stripe/saved-methods` branch will result in the `stripe` directory still existing.

It's probably a good convention to stick to at most one level. Consider if we had a branch `wip/stripe`, and we then wanted to create `wip/stripe/saved-methods`, we would get an error because we are trying to create a directory `stripe`, when there is already a file called `stripe`

When we have a branch with slashes in it, it gets stored as a directory hierarchy under `.git/refs/heads`.
