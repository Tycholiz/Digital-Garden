
##### Give user write access
`chmod u+w /usr/local/share/zsh`

##### Multiple users/groups
`chmod g+w,o-rw,a+x ~/example-files/`

##### Apply recursively to all children files/directories
`chmod -R ...`

##### Apply permissions set `rwxr-xr-x`
`chmod 755 test.txt`