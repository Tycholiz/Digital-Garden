
### Log
- we can pass a filename to `git log` to only show the history of a given file
- also, we can pass a commit SHA to see the revision history up until that point
	- This fact shows how by default, `git log` will run with `HEAD`, showing the revision history from the current branch's tip
- We can run `git log -n 4` to show only the last 4 commits from HEAD
	- equivalent to `git log HEAD~4..HEAD`
- running `git log master..origin/master` checks what exists on origin that doesn't on our local master. The opposite, `git log origin/master..master` checks what exists on our local master that doesn't on origin. If the output is empty, it means that our branches have not diverged.
