
## Gstatus
or simply `:G`
`<C-n>`/`<C-p>` - navigate files
`-` stage/unstage the file that you are hovering over
- can use visual mode

`p` - run `git add --patch` for the file under cursor
- splits file into hunks of changes and allows you to pick which parts to stage for commit
- not as useful, since we can just use `diffget` as described below.

`cc` - commit
`D` - open file under cursor as a diff
`U` - unstage a file ("I've staged a file and want to remove it from the index")
`X` - untrack a file ("I've changed a file in the working tree and want to discard all changes")
  - `git checkout -- <filename>`

`]c`/`[c` - navigate between hunks when in vimdiff window

## Gblame
`A` resizes to end of author column
`C` resizes to end of commit column
`D` resizes to end of date/time column
`<CR>` opens the patch that was applied by that commit

## Reconciling differences between working copy and index version
- `:Gw[rite]` - write (save) the current file and stage it
- `:Gr[ead]` - read the index version of the file into the current buffer
	- good for seeing what changes were made (that haven't been staged yet)

- `Gwrite` and `Gread` are opposites. Running `Gread` in the working file will do the exact same thing as running `Gwrite` in the index file +vv (both cause the working copy to revert to the index version)
  - Both of these commands reconcile differences between the working copy and
      the index version of a file by overwriting the contents of one buffer.
- We can be more granular with our reconciliation by using vim built-in `diffget`/`diffput` (alias `do`/`dp`, respectively. These aliases also run :diffupdate afterwards, to fix coloring issues)
  - While running `:Gdiff`, if we are in the index window and position the
     cursor over a hunk of text and run `diffget`, it will pull that hunk into
     the working copy of the file.
  - If there are changes in the working copy (right side) that you'd like to move to the index, you can visually select those lines and run `diffput`
    - this opens a new buffer with just the hunks you've added. we need to save this buffer before we `
  - `diffget` makes changes in the currently active buffer, while `diffput` makes changes in the inactive buffer (by drawing in differences from the active buffer)
![](/assets/images/2021-03-08-21-36-16.png)
  - run `:diffupdate` if colors get messed up as a result of doing this
- `:Gedit :0` - open index version of current file (remember: staging a file
    updates the index version of the file (?))
- `:Gremove` - wipe out the buffer and run `git remove` on the current file
- `:Gmove` - rename the current file
  - does a whole bunch more than `git move`, so it's not a direct replacement
  - ex. `:Gmove new-file-name` - (relative to the path of the current file)
- jump between conflicting regions with `[c`/`]c`

## Merge conflicts
- open a file with merge conflicts and run `:Gdiff`
	- might need to run `Gdiffsplit!` to get 3 windows
- The windows shown are:
	1. Target Branch - The branch we are merging into (normally master)
	2. Working Tree - The file as it currently is (with changes visible from both target branch and merge branch)
	3. Merge Branch - The branch we are introducing
- if using `diffget` and want to accept changes from the target branch (likely master), run `:diffget //2`. If we wan to accept changes from merge branch (likely feature-branch), run `:diffget //3`

# Diff
Left window is the commit we are comparing against, and right window is the current working copy

### Colors
red - lines were deleted
green - lines were added
teal - lines were modified

- knowing colors, we can easily notice if something has actually been removed, or if it was just moved/has a new structure
  - In this case, you'd find the green code in the left hand side, and try and match it up with a green chunk on the righthand side. If we find a match, then we know the code format just changed, and that no actual code was deleted.

## Seeing diff between 2 versions of a file
`:Gdiff <sha>`

`:Gdiff :0`
- diff between current file and staged version

`:Gdiff !~3`
- diff between current file and 3 commits ago


### UE Resources
- http://vimcasts.org/episodes/fugitive-vim-resolving-merge-conflicts-with-vimdiff/
- [cheatsheet](https://gist.github.com/mikaelz/38600d22b716b39b031165cd6d201a67)
