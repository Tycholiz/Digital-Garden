
When git comes across a file in your repo that is untracked, it will report it to you (in `git status`). However, if that file has been added to `.gitignore`, it will suppress it.
- once a file is known to git (ie. it has been in the index), adding the file to `.gitignore` has no effect (ie. the file will continue to be tracked)

if you need to stop tracking a file (for example, we've already added the file to the index, but now want to start ignoring it), we need to run `git rm --cached <file>`
- for folders, `git rm -r --cached <folder>`
