
### Take commits from 6f3e5a (inclusively) until fe85ab, and plop it on top of `HEAD`
`git cherry-pick 6f3e5a^..fe85ab`
- `^` gets the parent, to make the range inclusive
