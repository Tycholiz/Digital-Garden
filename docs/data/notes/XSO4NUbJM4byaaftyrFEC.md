
##### See commits that exist on a given branch
`git log feature-branch`

##### See commits that exist on a branch, while excluding ones that live on another
`git log feature-branch ^main`
- equivalent to `git log main..feature-branch`

##### See the history of a file across this rename
`git log --follow -- /path/to/file`

##### See only commits where files were deleted
`git log --diff-filter=D --summary`
