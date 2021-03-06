
# Overview
From a feature branch (FB), if we rebase master, conceptually we are rewinding back to the point where the FB split from master and updating that point with master and then “replaying” the FB commits on top of that
- Therefore, rebase is an alternative way to get commits from a feature branch onto a master branch
	- with `git merge`, we take all of the commits on the feature branch and try and jam them together with the commits of the master branch, in order to make a new commit (the merge commit).
	- with `git rebase`, we take all of the commits from the current branch and move them on top of another. This involves rebasing first, then doing a regular merge (which will be fast-forward, since the base commit will now be a direct ancestor)
		- When we run `git rebase master` from our feature branch, we say "hey master, I want to take all the work I've done so far and make it look like it was built directly on top of all of the work that *you* have"

If you're `rebase`ing, you will always have to `--force` when you push, because rebase rewrites history— That's simply how it works. Rewritten history isn't going to fast-forward on top of that same history by definition.
- only the commits that are being replayed are rewritten (and thus, new SHAs). If we are on FB and rebasing on top of master, the commits from master will remain unchanged (ie. history isn't being written here). However, the commits that only existed on FB will be rewritten, and thus will have new SHAs.
- While collaborating with others using the "rebase workflow", you should always `git pull --rebase` to avoid the circumstance where a merge commit is made. If one person rebases, and then pulls that code with merge commits, it will be a difficult rebase (spec:) due to the rewritten history.

As the name suggests, rebase exists to change the base of a branch (ie. the origin commit). We do this by replaying a series of commits on top of a new base.
- This is mostly needed when a series of local commits is deemed to start from an obsolete base (put another way, our local master is very out of sync with origin/master)

- behind the scenes, git is duplicating the commits of the feature branch, putting them on top of the master branch, and then blowing away those original feature branch's commits (hence why they are greyed out in the following image).
	- therefore, in a sense it is rewriting history, as evidenced by the fact that the duplicated commits have a different SHA than the originals

- Because rebase rewrites history, it's important that we pull all remote changes to our local master branch before rebasing, so that we are reanchoring our feature branch's commits to the current version of the code.
- ex. we are on branch `about`, which has diverged from `master`. We want to incorporate changes from master into `about`. From `about`, we run `git rebase master`:

## Process
Git always squashes a newer commit into an older commit or “upward” as viewed on the interactive rebase todo list, that is into a commit on a previous line.
- This means if commit1 is a `WIP` commit, and commit2 is the one we want to keep (along with changes from commit1), then we must actually `squash` commit2 into commit1. Doing so will allow us modify the commit message (now a combination of the messages from commit1 and commit2) before rewriting the history.


### Behind the Scenes
1. Git will checkout the upstream branch and copy all the changes you've done since you last merged, placing them on the tip of the upstream branch.
	- ex. in the above image, to an outside observer it would seem that you had checked out the upstream branch from ***a***b and then done your changes.
	- note: here upstream most likely is origin/master or simply master, but it could be any branch we are "merging" into.
2. Git produces a series of patch files of your work and starts applying them to the upstream branch.
	- consider that these commits are actual copies with different commit SHAs

### Process
1. when finished with feature branch, pull all remote changes onto master
	- if local master === origin/master, step 2 can be skipped, since it would have no effect anyway
2. from feature branch, run `git rebase master`, which will cause our feature branch commits to be anchored against the updated master branch
	- consider that when we checkout a new branch, we have a common base with the branch which we checked out from. Rebasing master here ensures that the remote changes that happened and got merged into master (remotely) are included as part of that anchor.
3. from master branch, run `git rebase feature-branch`, copying and placing the commits of the feature branch onto the main branch.

#### Conflicts
- say we are rebasing 8 commits onto the new branch — each one could cause a conflict, and we can resolve the conflicts introduced by each commit one by one.
	- fix the file, run `git add`, then run `git rebase --continue` (which moves us on to the next patch, until all are completed)

### When to use rebase
1. Any time we are working on a long term branch that needs to stay somewhat up to date with master. It is better to keep it as up to date as possible, rather than staying diverged for a long time.
	- ex. Imagine working on an experimental branch and getting blocked at some point. This is a scenario that would cause your branch to diverge from master more and more over time. When it does come time to pick up work again, it would be a good time to rebase to the master branch. The result is that it would look just like you started from there.
2. Consider that in a perfect world, my coworker and I would have a linear commit history (even though we are developing asynchronously, it makes more sense looking back if we have a straight line of commits). In this ideal world, I would be developing my work off the base of my coworker's work, and vice versa.
3. Imagine we have a *quick-fix* branch that we don't want muddying up the history. If master has not been touched since we branched, the ff merge is automatic. However, if master has indeed changed, then we need a way to tweak *quick-fix* so it becomes a direct descendent of *master* again.
	- In this scenario, we want our local master to have the same tip as origin/master. This would allow us to do a ff merge, thus avoiding muddying up history.

### Drawbacks to rebase
- doesn't play too well with open source projects, since it becomes hard to trace changes introduced to a codebase.
- doesn't work well when working on a shared branch, since commits are rewritten.
- only rebase when working on a local branch prior to pushing, or on remote repos where you are the only contributor (ex. for backup purposes).
	- In the second scenario, we'll need to force push (since we replaced its commit history with a fresh one).
	- Issues arise when other people pull in objects that were orphaned by the rebase process.

#### Shared branches
Rebase is not a great candidate for shared branches. Because `git push --force` is a fact of life to the "rebase-way" of Git workflow, we would have to be careful to check if someone else has pushed to the remote branch first. This is why we should use `--force-with-lease`, so that we cannot overwrite commits that have been pushed already to that remote branch. If there have been, we will get errors, and we can `git pull --rebase` to incorporate those changes, before force pushing again.

## Long-lived Feature Branches (LLFB)
as the LLFB gets periodically rebased off master its commits get rewritten, so we end up with different SHAs for "the same" content of the commit.

# E Resources
[VS Code tip: Interactive rebase editor from the GitLens extension](https://m.youtube.com/watch?v=P5p71fguFNI)
