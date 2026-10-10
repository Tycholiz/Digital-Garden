
HEAD is the pointer that points to the latest commit on the currently checked out branch.
- Therefore the HEAD is what lets Git know which commit will be the parent for the next commit.

- spec: While a branch simply points to a commit, a HEAD simply points to a branch (or a commit). If it points to a commit, it is because the commit is not at the tip of its branch, resulting in a detached HEAD.
	- Recall that when HEAD does not coincide with the tip of any branch, the result is a detached HEAD. From Git's point of view, this simply means that HEAD doesn't contain a local branch.

Under normal circumstances, HEAD points to the branch we currently have checked out. However, we can also checkout a commit. Interestingly, we can checkout the same commit that our branch points to (which would be a detached head). In this case, both working trees would be identical. 

Normally, the HEAD file is a symbolic reference to the currently checked out branch. We can see this by logging out the contents of `.git/HEAD`
- here, symbolic reference means that it contains a pointer to another reference.
