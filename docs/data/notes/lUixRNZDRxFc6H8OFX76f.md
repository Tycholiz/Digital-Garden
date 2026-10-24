
#### Set your local branch to track a remote branch
`git branch -u origin/dev`

#### push to new upstream
tip: use with `--force` to overwrite everything remotely
`git push -u origin dev`

#### Get origin URL
`git config --get remote.origin.url`

#### Add remote URL
`git remote add origin git@github.com:USERNAME/REPOSITORY.git`

#### Remove remote
`git remote rm origin`
