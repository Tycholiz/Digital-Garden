
`-l` - only print out the filenames that have the pattern
`-r` - recurse, allowing us to run grep on directories

- grep for lines with *pattern1* while filtering out lines with *pattern2* from *file.txt*
    - `grep pattern1 file.txt | grep -v pattern2`
