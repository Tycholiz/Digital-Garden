
### Find out which branch a commit took place on
If within last 90 days (ie. before gc), check [[reflog|git.reflog]]
`git reflog show --all | grep d7f32e`

`git name-rev --name-only d7f32e`
