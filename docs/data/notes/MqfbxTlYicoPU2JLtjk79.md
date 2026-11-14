
To undo a merge, simply run `git reset --hard HEAD`
- by default, only the index will be reset, meaning the partially merged files will remain in the working directory.
- If we've merged and committed already but want to revert the merge, we can discard the commit with `git reset --hard ORIG_HEAD`

From a DX standpoint, the big difference between rebase and merge is that `merge` will preserve the "railroad" switch that was our branch when we look at the history, whereas `rebase` will show it linear.
- "Should this branch remain visible in the graph?"

[git merge with multiple parents](https://softwareengineering.stackexchange.com/questions/314215/can-a-git-commit-have-more-than-2-parents)

## When to use Merge
1. If master remained untouched while we were working on our feature branch (in which case a fast-forward merge would be automatic).
2. When we have work that is related to an agile ticket or bugfix (which would allow us to look back in history and clearly see it)

## Terminology
The `ours` `theirs` terminology is from the perspective of the "already existing branch". Put another way, if we are on main and merging in a feature branch, main is `ours` and feature is `theirs`. Also in the same way (and using a different paradigm), `current change` would refer to main, and `incoming change` to feature.

This is also the same as with rebase. if we are rebasing main onto feature, then `ours` will refer to main (`git rebase main` from feature branch)
