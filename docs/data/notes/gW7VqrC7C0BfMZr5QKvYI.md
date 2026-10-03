
## \copy
- This is the client-side version of the copy command. It gives us a distinct benefit, which is the source file is required on the client side. In other words, we can execute the copy command on the same machine as where the source file is stored; we don't need to upload the file to the postgres server first.
- copy is much faster than running `insert into...`

