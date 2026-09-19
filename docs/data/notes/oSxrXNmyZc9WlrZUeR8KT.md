
In general, git reset is used to move branch tips around (likely to another commit)
- `reset` is the opposite of `add`

If we pass a filename to `git reset`, then the staging area will be updated to match the given commit instead of the working directory (the branch pointer does not move).
- ex. if we have 3 files staged, we can remove one of them with `git reset HEAD index.js`, making `index.js` match the version found in HEAD. The working directory and current branch are left alone. The result is a staging area that matches the most recent commit and a working directory that contains the modified `index.js` file.
	- Put another way, we are unstaging the file.

![8f6b8c503feeeceab0f2175850b7acbd.png](:/78999c9edf5c4f129b389d40acc423cc)

## Soft
- moves the HEAD to the provided `<SHA>`, while keeping working tree and staging area intact.
- *soft* means the commit is canceled and moved before the `HEAD`
- `git reset --soft HEAD^` - undo last commit

## Mixed
- moves the HEAD to the provided `<SHA>`, while keeping working tree intact.
	- Move the HEAD backward `<n>` commits, but don’t change the working directory.
- reset the staging area
- `git reset <file>` is the opposite of `git add <file>`
- ex. if we are on c3 and do `git reset c1`, we will go back to c1, and the working directory and index will remain unchanged
    - This is the default type of reset

## Hard
- moves the HEAD to the provided `<SHA>`, keeping neither the working tree nor staging area intact.
	- Move the HEAD backward `<n>` commits, and change the working directory to match
- the only version of `reset` that actually results in a changed working directory file.
- ex. if we are on c3 and do `git reset --HARD c1`, we will go back to c1 (i.e. our head will point to c1) and c2 and c3 will be "destroyed", and working directory wiped.
