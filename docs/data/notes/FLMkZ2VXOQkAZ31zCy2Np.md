
# Amend

* * *

## Operators

### ~ Operator
- allows us to reach parent commits
	- ex. `git show HEAD~2` shows the grandparents of HEAD
- if the commit has more than 1 parent (ie. merge commit), then the first parent of the commit will be used by default.
	- Need to use `^` to specify a different commit

### ^ Operator
- allows us to specify which parent we want to refer to
	- spec: therefore, if there is only one parent, then `HEAD~1` refers to the same commit as `HEAD^1`
![](/assets/images/2021-03-07-22-45-05.png)
