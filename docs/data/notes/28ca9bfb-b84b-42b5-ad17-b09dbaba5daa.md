
# Remote
- a pointer to a branch on a copy of the same repository.
	- remote simply means a copy of the repo on someone else's machine.
- *origin/master* means "the master branch of the origin remote"
- When you clone a repository, Git automatically adds an origin remote pointing to the original repository, under the assumption that you’ll probably want to interact with it down the road
	- run `git remote -v` to see what origin is
- we can run `git branch -r` to see the remote branches available to us. If there are none, then we can run `git fetch <remote-name>` to copy them over.
- Checking out a remote branch takes our HEAD off the tip of a local branch, resulting in a detached HEAD:
![efafe6e1a14641006f80ee5a895572b2.png](:/287b1c5127484a3b99356dfdfa60acbc)

## Upstream
- Imagine we forked a repo remotely, then forked it locally. `Upstream` would be the original repo that we forked, and `origin` would be the remote repo of our forked version
- upstream means "towards the trunk" (ie. towards the single source of truth)
- By default, `origin/master` is set as the upstream branch of `master`, so `git pull/push` will default there.
- `git branch -vv` <-- show upstream branch of local version.

## Tracking Branch
- The local branch that is connected to a remote branch.
- ***ex.*** `master` **==>** `origin/master`
- checking out a remote branch from the local repo will create that branch.
- `git branch --remotes`
- `git remote -v` - list all remote repos you've connected to

#### Get URL of remote
`git remote get-url origin`