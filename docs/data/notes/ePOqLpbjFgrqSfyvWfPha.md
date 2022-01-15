
### Blob
- A particular version of one file
	- the only part of a file that is relevant to a blob is the file's contents. The object database does not care about the filename.
		- ex. if we write 2 different files with the same content to the object database, only one SHA will get recorded, since the same content hashed will always produce the same result.
			- the same can be said for a tree object, but not for a commit object, since the author and time of commit makes their content always unique.
- the blob itself doesn't have a name, but it is referred internally by the hash of its content
	- the "hash of its content" part of this last point means that as long as 2 different pieces of content are identical (ie. file contents are identical), the resulting hash will also be identical. This also illustrates the fact that blobs are decoupled from a (file)name, as it has no bearing on the produced hash.
