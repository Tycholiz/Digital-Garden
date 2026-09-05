
### Patch file
The default output of `git diff` is a valid patch file, meaning we can pipe its output into a file, give that file to someone else, and they can apply it with the `patch` command.
- ex. `git diff master..experiment > experiment.patch`. The recipient can then run `patch -p1 < ~/experiment.patch`

Each patch represents a full commit, complete with metadata like author and date.
- ex. If we have made 2 commits since master, then running `git format-patch master`

### range-diff
`git range-diff` allows us to inspect how two commit ranges are different

## Under the hood
to compare 2 commits, Git starts by looking at the root trees of each commit (which are almost always different). Then, Git initiates a depth-first-search on the subtrees by following pairs when paths for the current tree have different OIDs.

* * *

- `git diff` - show changes between index and working tree
- `git diff --staged` - show changes between index and HEAD (ie. last commit)
- `git diff master..feature-branch` - show changes between master and feature-branch 
	- `git diff f733ed..` - show changes between a commit and HEAD
- `git diff -- package.json` - show only changes in a specific file.

