
##### See the patch of the current commit
`git rebase --show-current-patch`

##### Status of rebase
Number of commits in this rebase
`cat .git/rebase-apply/last`

Out of the number of commits in this rebase, which are we on?
`cat .git/rebase-apply/next`

Which commit is currently being applied?
`cat .git/rebase-apply/original-commit`

#### Cherry-Pit: Remove a specific commit
`git rebase -p --onto SHA^ SHA`

##### alias cherry-pit
usage: `git cherry-pit <SHA-to-remove>`

#### Include some changes as part of a previous commit
Imagine we realized that we should have included a change (perhaps deleting some old comments) as part of a previous "cleaning commit"
Of course, we will have to change history to do so, with `rebase`:

##### Stash method
Strategy: stash the desired changes, reset back to a commit, pop those changes and amend commit, then complete rebase

1. Use `git stash` to store the changes you want to add.
2. Use `git rebase -i HEAD~10` (or however many commits back you want to see).
3. Mark the commit in question (a0865...) for edit by changing the word pick at the start of the line into edit. Don't delete the other lines as that would delete the commits.
4. Save the rebase file, and git will drop back to the shell and wait for you to fix that commit.
5. Pop the stash by using `git stash pop`
6. Add your file with `git add <file>`.
7. Amend the commit with `git commit --amend --no-edit`.
8. Do a `git rebase --continue` which will rewrite the rest of your commits against the new one.
9. Repeat from step 2 onwards if you have marked more than one commit for edit.

##### Autosquash method
1. Stage the changes that we want to include in a previous commit
2. Create a new commit with an identical message of the original commit with the following command
`git commit -c <ORIGINAL-COMMIT-SHA>`
	- pro-tip: use `HEAD~2` syntax for relative
3. With the editor open, prepend the name of the new commit with `fixup! ` (or `squash! ` if we want to edit the commit message)
`fixup! refactor: clean up payment methods`
4. run `git rebase -i --autosquash <SHA>`, where `<SHA>` is the commit immediately before the original commit we want to amend.
5. Git will detect the `fixup! ` directive, and will look for the commit with the same message. With this information, Git will perform the squash directive (combining the commits), and then continue to rebase the rest of the commits

##### fixup (manual)
1. make the commit `git commit -m fixup`
2. run `git rebase -i`
3. move the commit, and squash it into an earlier one

##### git fixup alias
```
fixup = "!fn() { git commit --fixup ${1} && GIT_EDITOR=true git rebase --autosquash -i ${1}^; }; fn"

// then add staged changes to commit 3 before HEAD:
git fixup HEAD~3

```
