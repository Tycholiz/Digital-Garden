
# Checkout
- The act of switching between different versions of a file, commit or branch
- Think of it as switching between snapshots
- When checking out a different branch, Git makes your working directory look like that branch. Any checked in content that is in your working directory but is not in the new tree will be removed. This is why Git only lets us checkout another branch if everything is checked in (ie. no uncommitted modified files).
	- The reason for this is that Git will remove files that are not necessary in the branch we are checking out.
- if we add a file path to `git checkout`, only the specified file will be checked out, and the branch pointer will not be updated.
	- ex. `git checkout HEAD index.js` will check out the most recent (HEAD) version of `index.js`
![0a0ed0120ebc145ec651db96de7b73c4.png](:/a7a7bd8f7ee646d4a308c17366095fad)

Checking out a file
- running `git checkout <file>` is similar to running `git reset <file>`, except checkout updates the working directory, while reset updates the staging area.
	- This has a similar effect to `git revert`, with an important difference: `revert` only undoes changes introduced the commit, while `checkout` undoes all changes *since* that commit.
	- ex. what if we want to change the file in the working tree to what it was 2 commits ago. We can run `git checkout HEAD~2 <file>`.
- while running `checkout` on a branch/commit will move the HEAD reference, running `checkout` on a file will not, meaning we don't change branches.
