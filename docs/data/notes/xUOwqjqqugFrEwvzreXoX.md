
#### Remove a file permenantly from the history
tip: check the resulting SHAs and compare the commit messages of before and after. Do the SHAs differ from what you would have expected? Between the two, are SHAs of "same" commits different BEFORE (ie. earlier in git history) the file you are removing (e.g. here `.env`)
`git filter-repo --force --invert-paths --path .env`
