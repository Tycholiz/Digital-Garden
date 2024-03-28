
An inode is a data structure that describes a FS object (like a file/directory)
- an inode is a [[virtual file-system|fs.virtual]] entity
- the inode stores metadata about the object, such as its disk block location, file size, file type 
- it also stores owner/permission data about the object

In reality, a directory is just a list of names that are each assigned to an inode
- A directory contains an entry for itself, its parent, and each of its children.

- on a UNIX system, files are user facing (ie. we work with them directly). The structure of a file exists only as a virtual FS entity in memory (there is no phsyical correspondent of it)
- From the point of view of the underlying filesystem, the inode abstracts away the files that the user would directly interact with. From the point of view of the user, the file abstracts away the inode.
- stands for *index node*

## Dentry
The dentry (directory entry) associates an inode with a file name
