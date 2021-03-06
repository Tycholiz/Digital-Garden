
# Boundary Commit
A boundary commit is the commit that limits a revision range but does not belong to that range. For example the revision range `HEAD~3..HEAD` consists of 3 commits (HEAD~2, HEAD~1, and HEAD), and the commit HEAD~3 serves as a boundary commit for it.
- This is related to the concept of inclusive/exclusive ranges. In this comparison, the boundary commit would be an exclusive boundary.

More formally, git processes a revision range by starting at the specified commit and getting at other commits through the parent links. It stops at commits that don't meet the selection criteria (and therefore should be excluded) - those are the boundary commits.
[Source](https://stackoverflow.com/questions/42437590/what-is-a-git-boundary-commit)
