
# Tar

### Compress a folder into a tarball
`tar -czvf output.tar.gz input optional_input2`

### Uncompress a tarball using a gzip compressor
`tar xvzf file.tar.gz`

### Send to different directory than current
`tar xvzf file.tar.gz -C ~/Downloads`

spec: it seems that we have to cd into the directory that we will create the tar from. If we don't, then the resulting tar will take the whole absolute directory along with it, and when we uncompress it, we will get something like `/Users/kyletycholiz/tarred-file`
- The same can definitely be said for `zip` utility
