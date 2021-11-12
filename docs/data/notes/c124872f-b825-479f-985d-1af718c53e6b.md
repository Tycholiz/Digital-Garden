
# Fetch
- running fetch will pull all refs and objects that we don't already have from the remote repo, and will put them into the object database. 
- fetch is opposite of push, in the sense that fetch will import branches, while push will export them
	- when we run `git push origin master`, we are exporting our master branch to origin. If we were to run `git branch` on origin, we would see our local branch that we just pushed, listed as a local branch.
		- ex. Imagine we had a repo called `foo`, then cloned in locally into a different directory on our machine and called it `bar`. On `bar`, origin would default to `foo` (since it was copied from `foo`). `foo` would not have an origin by default, since it did not come into existence by being copied. We could manually add `bar` as a remote. Now, if we made a branch on `foo` and pushed it, that branch would be available locally on `bar`.
