
There are 3 permission groups: Owner, Group, Other
- each permission group has 3 permissions, called a permission set
	- ex. `rw-` is a single set
- each file/directory has 3 permission sets— one for each permission group.
- rwx mean different things if we are referring to a directory or a file
	- `r` 
		- on file means we can read the contents 
		- on directory means we can run `ls`
	- `w` 
		- on file means we can modify file contents
		- on directory means we can add/delte files 
	- `x`
		- on a file means we can run it
		- on a directory means we can `cd` into it 


### File ownership
- every file is owned by a specific user (`UID`) and a specific group (`GID`)
	- `chown` is used to change both
		- ex. `chown <user>:<group> test.txt`
- each member can belong to many groups (`/etc/group`), though a user can only have one primary group (`/etc/passwd`).
	- run `$ id` to see the groups the current user belongs to
	- When a user creates a file, the file will be owned by the primary group
- similar to how we need to source the `.zshrc` before changes are live, we need to log out and log back in before group membership is "activated"

### Settings Permissions
The numerical method is quite easy. For example, we can just replace each `rwx` set by it's binary positional value (from RTL: 1, 2, 4, 8, 16, 32...) and add the the numbers in each set.
```
-(rw-)(rw-)(r--)
-(42-)(42-)(4--)
664
```
- By this, we can define a `7` as `rwx`, a `5` as `r-x`, and so on

* * *

Set new files/directories in a subdirectory to follow the group ownership of the specified directory
- `chmod g+s /var/www/my-project`

[Unix permissions guide](https://support.plex.tv/articles/200288596-linux-permissions-guide/)
