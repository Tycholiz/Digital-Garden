
The Reflog gives a chronological account of how HEAD has moved. Each time it moves, there is a new entry in the Reflog.

The reflog is a list of chronological hashes, which represent where you have been during commits, all without regard to branches.
- Since branches don't matter, we are able to recover dangling commits.
- Each time a branch is updated to point to a new reference (ie. HEAD changes), an entry is written in the reflog to say where you were.
- Since the branch is updated whenever you commit, the git reflog has a nice effect of storing your local developer's history.

It serves as Git's safety net, recording every change made in the repo (regardless of whether it was committed or not)
- The listed commit hash represents the HEAD after that action.

Alongside the caret (`^`) and tilde (`~`) operators, git provides the `@` for referencing reflog entries in a relative way.
- `git show HEAD@{5}` gets us the value of HEAD 5 movements ago.
- we can also see where the HEAD was by day:
	- `git show HEAD@{yesterday}`
	- `git show HEAD@{1.day.ago}`

While `git log` shows a history of the commits, we can think of `git reflog` as showing us a history of everything (what branch we checked out, what commands we ran etc.)

Becuase it keeps a full history, even if we were to `git reset`, we can still access the commit SHA
- ex. we made some commits, then reset. If we decide that we want to *undo* the reset, all we need to do is checkout the commit we want with `git checkout HEAD@{1}`
```
ad8621a HEAD@{0}: reset: moving to HEAD~3
298eb9f HEAD@{1}: commit: Some other commit message
bbe9012 HEAD@{2}: commit: Continue the feature
9cb79fa HEAD@{3}: commit: Start a new feature
```
- This puts us in detached head state. All we need to do is create a new branch, and continue working on our feature.

#### Data Recovery
- Most of the time, the reflog is our friend in the circumstance where we want to recover data that has been "lost"
	- we can either run `git reflog`, or use `git log -g` which gives us normal log output for the reflog
- Imagine we hard reset a number of commits, effectively "erasing" them from the git log. All we need to do is find that commit with `git log -g`, create a new branch with its SHA (`git branch recover-branch <SHA>`)
- If the data we are looking for is not in the reflog, we can try using `git fsck --full`, which will list all objects that aren't pointed to by another object.
