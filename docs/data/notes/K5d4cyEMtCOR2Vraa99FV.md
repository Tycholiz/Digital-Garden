
### Ref
- In a world where all we have is [[blobs|git.inner.objects.blob]], [[trees|git.inner.objects.tree]] and [[commit objects|git.inner.objects.commit]], we would have to remember the commit SHAs so we have a starting point for our history. Instead it would be easier if we had a file where we could store that SHA under a simple name, so we could use the name instead of the SHA. These simple names are called *refs*.
- A ref is anything pointing to a commit.
    - ie. branches (local/remote) and tags, mostly.
    - unlike objects, they are mutable and constantly change.
		- In Git, the immutable parts are the expensive ones (ex. an entire blob), while the mutable parts are just references, and therefore cheap (ex. branch, remote, HEAD)
	- Think of it like a user-friendly alias for commit SHA
		- ex. `my-feature-branch` is actually an alias for the branch. In reality, the branch name is a SHA
- `git show-ref`
	- ex. if we look at `.git/refs/heads/master`, we will see a commit SHA, which is the latest commit on this branch. If this wasn't done automatically for us, we could manually create this file with the SHA, which would effectively be saying "hey Git, when we are on master branch, the latest commit will be this SHA"
		- Therein lies basically what a branch is: a simple pointer to the latest commit
- stores pointers to commit objects that we would consider to be significant
	- ie. the ref is the location of the commit 
    - The head commit of each branch that exists (`refs/heads` directory)
    - The head commit of each branch on the remote repos (mostly just `origin`)
    - stash commits
    - tags
- if commits point to trees, and trees point to blobs, then refs conceptually point to commits
