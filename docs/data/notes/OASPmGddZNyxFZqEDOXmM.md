
# Overview
Cherry-pick works by copying a commit diff (between it and its parent) onto the current branch

cherry-picking results in the creation of a new commit that has an identical diff to the diff between the specified commit and its parent. When we run cherry-pick, the changes that were introduced in a single commit will be applied on top of HEAD.

Instead of working bottom-up (or past to the present) like rebase, cherry-pick works top-down (or present back to the past)

When you cherry-pick a commit, you also specify which parent commit to consider, with the `-m` parent-number argument. The cherry-pick command then generates a diff against that parent, so that the resulting diff can be applied now.

Should you choose to cherry-pick a non-merge commit, there is only one parent, so you don't actually pass `-m` and the command uses the (single) parent to generate the diff. But the commit itself is still a snapshot, and it's the cherry-pick command that finds the diff of commit^1 (the first and only parent) vs commit and applies that.

If you want something else done aside from applying the patch between a commit and it’s parent, then you don't want the cherry-pick command. For instance, if you just want a particular snapshot's version of some file, use `git checkout <revspec> -- <path>`, or `git show <revspec>:<path>`

It's crucial to recognize that cherry picking does not involve moving a commit on top of HEAD. Rather, it involves copying the diff produced by 2 commits, and applying that patch on top of HEAD. This process results in a new commit SHA.

use cherry-pick when you want to rebase, but have more power over what exactly you want to bring over from one branch to another

### Resetting and Cherrypicking
This lends itself to a strategy that is similar conceptually to `rebase`

Imagine we have 2 branches that have a common base, but have diverged. We want to get a series of commits to be placed onto the tip of an existing branch (much like rebase)
1. from feature-branch, `git reset main --hard` to the HEAD of the master branch. These branches are now identical.
- Before doing this, we need to know which commits we want. It's probably even a good idea to duplicate the branch as a backup just in case.
2. cherry pick the range of commits on top of feature-branch.
- as a result, the only conflicts will be in the feature-branch commits.
