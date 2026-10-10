
Find and Replace a pattern in all files of a tree
- find and replace all occurrences of `foo` with `bar` within a directory tree
	- `grep -rl 'foo' . | xargs sed -i .bk 's|foo|bar|g'`
	- the first part gets a list of all files that have the pattern `foo` in it, then it pipes that list into the second part, which runs the sed substitution on each file
	- if we don't want to create a backup, then replace .bk with ''

Find and Replace a pattern in certain files
- find and replace all occurrences of `foo` with `bar` in all .js files
	- `find . -name "*.js" -exec sed -i '' s/foo/bar/g {} +`
