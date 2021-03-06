
- `git push origin master` moves the HEAD of the central repo:
	- This has the exact same result as if we went on the central repo and ran a fetch and fast-forward merge.
![cbbedd3aff8b9bae62000af2a996468e.png](:/1f2474a7a067490f91335052cb37b7cf)

## Avoiding race conditions with force push
What if we want to force push, but don't want to run into problems if someone else has pushed to that branch in the meantime?
To get a warning when trying to force push to a branch that has been committed to in the meantime, run:
`git push --force-with-lease`

## Force push
- Force pushing to feature branches is a fact of life. Force pushing to a `master` branch should be considered with extreme care.
	- Force pushing to feature branches allows us to have a clean history of commits on that feature branch. If we embrace `rebase`-`merge` instead of just `merge`, then we will encounter lots of scenarios where we must force push to that feature branch.
		- Note: this strategy should NOT be taken if multiple developers are working on the same branch. Rebase is not a good candidate for shared branches.
