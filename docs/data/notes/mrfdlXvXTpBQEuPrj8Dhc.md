
Diff between stash and HEAD (ie. view the changes tied to the stash)
- `git stash show -p stash@{1}`

Retain staged work of stashes
- run `git stash pop --index` so that staged files return as staged when you pop the stash

Reverting `stash apply`
- `git reset --hard`
	- assuming you had everything in a clean state when you started

Popping stash onto a new branch
- `git stash branch <branch-name> stash@{0}`

Adding a message
- `git stash save "<message>"`

Including untracked files with stash
- `git stash --include-untacked`

Deleting a stash
- `git stash drop stash@{1}`
