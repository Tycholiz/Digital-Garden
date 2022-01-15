
### Flow
From the commit that is not working,
1. run `git bisect`
2. run `git bisect bad` to indicate the current commit is "bad"
3. run `git bisect good <SHA>` on a commit we know works
4. git will checkout a commit in the middle of the good-bad range. See if this version works, and run `git bisect good`/`git bisect bad`
5. repeat until we find the commit that introduced the issue
6. run `git bisect reset` to clean up bisection state, returning to original HEAD

* * *

you can pass git-bisect a script which programmatically tests a tree for the presence of a bug and avoid false positives
