
### Push new changes to PR branch that has diverged.
1. Before pushing any new changes, rebase `master` onto the feature branch, and resolve conflicts:
`git rebase master`
2. on feature branch, run `git push --force`
	- If we don't force push, we will get a warning that branches have diverged and we must first pull. This is a bad idea, spec: because we would be getting rewritten history, and would in effect be introducing many more commits than we are actually intending.
