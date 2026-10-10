
## Merge conflicts with `git revert`
Imagine we make CommitA, add CommitB and CommitC, then attempt to revert CommitA, and we have a merge conflict.

In this circumstance, the terms "current changes" and "incoming changes" refer to conflicting changes between what is in your working directory (the current state of your files) and the changes that the git revert command is trying to apply (the changes from the commit you're reverting).

### What Happened:
*Made Changes & Committed*: You modified a file, committed those changes, and then continued to make several more commits after that.
*Reverting the Commit*: When you tried to revert that particular commit, Git attempted to apply the inverse of the changes made in that commit to your working directory. However, because you made further changes to the same file in subsequent commits, a conflict arose.
### Merge Conflict Reason:
*Current Changes*: This refers to the state of the file in your working directory based on all the commits you've made since the commit you're trying to revert.
*Incoming Changes*: These are the changes that git revert is attempting to apply, which are the inverse of the changes made in the commit you're reverting.