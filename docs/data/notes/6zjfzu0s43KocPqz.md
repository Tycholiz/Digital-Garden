
#### Match a pattern only in UNSTAGED hunks
`git diff -U1 | grepdiff 'TODO' --output-matching=hunk`

#### Match a pattern only in STAGED hunks
`git diff -U1 --staged | grepdiff 'TODO' --output-matching=hunk`

#### List the files that match a pattern in a given git range
Here, look for the pattern `TODO` in all commits from `main..HEAD`
`git grep TODO -- $(git diff --name-only main..HEAD)`