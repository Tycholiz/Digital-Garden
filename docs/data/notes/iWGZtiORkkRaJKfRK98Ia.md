
<!-- This file should probably be refactored so that we include each recipe along with whichever submodule it belongs to (ex. deleting commits should probably be in git.cli.rebase.cook) -->
<!-- Is this the most logical place that we'd expect to find this information? -->

### Rewording/deleting commits
- imagine we want to change a commit message from 2 commits ago, or imagine we want to delete it altogether
	1. We can run `git rebase -i HEAD~2`
		- meaning "I want to operate on the last 2 commits"
	2. vim will open and will list all of the commits that we have asked to change.
	3. change the command from `pick` to `reword` (or `drop` to delete) of the relevant commit, then save and close the file.
	4. change the message, save and close.

### Changing contents of commit/Splitting commits
1. run `git rebase -i HEAD~n`, where `n` is the commit immediately preceding the commit we want to edit.
3. change the rebase command of the commit we want to edit from `pick` to `edit`
4. run `git reset --mixed HEAD^`, which leaves the working directory unchanged, but reverses the commit
	- `HEAD^` is the parent of our current commit
6. do our normal operations, adding files, deleting files, or modifying them as we please. `git add` them, `commit` them as per normal workflow.
7. run `git rebase --continue`

### Reverting commits
- think of `git revert` as an inverse operation to `git commit`. Effectively, this command creates a new commit that undoes all of the changes introduced by a certain commit.
- Imagine we made a commit that simply changed which port our app connects to. Later on down the line, imagine that we want to "undo" that, and go back to the original port. We could change that port in the code and commit it, or we could simply run `git revert <SHA>` to make a new commit, known as a `revert commit` whose sole purpose is to reverse the changes that that particular commit actually made.

### Squashing commits
- Imagine we made 3 commits that should logically only be one.
	1. run `git rebase -i HEAD~3`
	2. leave the commit that everything will get squished into alone, but change the commits that will be thrown away from `pick` to `fixup`
		- listed by oldest to newest
		- fixup and squish are similar, but fixup will discard the commit messages of the discarded commits.
		- if we want to retain the commit messages, then use `squash` instead of `fixup`
- If we compare the git logs before and after, we will notice that the SHA of the commit with the same message will be different, showing that we are in fact rewriting history

### Splitting commits
- Imagine we have made a commit that realistically should actually be split into 2 commits ("add navbar and fix bug")
	1. run `git rebase -i HEAD~2`
	2. change the commit we want to split from rebase operation `pick` to `edit`
	3. after saving the file, git will put us onto a special rebasing branch
	4. unstage all files that were added during that commit by running `git reset HEAD^`
	5. perform normal workflow, by adding commit1 changes and committing, then adding commit2 changes and committing
	6. run `git rebase --continue`

## Undoing Work
- this will discard everything permanently

### Restoring to the state of the last commit
- if just a single file, we can simply run `git checkout HEAD <file>` to blow away all changes, and restore the file to what it was in the most recent commit.
- if we want to blow away all changes and get the exact state of the last commit, we can run `git reset --hard HEAD`
	- This tells Git to replace the files in your working copy with the "HEAD" revision
	- we can replace HEAD with any SHA to go back to that previous version.
	- note: this will not produce any new commits (like revert), nor will it delete any old ones. Instead, it works by resetting your current HEAD branch to an older revision (also called "rolling back" to that older revision)
	- note2: since this doesn't rewrite history, the commits we "erased" are still available
