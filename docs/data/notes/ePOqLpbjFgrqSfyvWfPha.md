
A blob is the content encapsulated within a single file.
- note: this does not include the file itself. Therefore, a blob has no notion of filename nor permissions (handled by the [[tree|git.plumbing.objects.tree]] object).

The blob is stored internally by the hash of its content
- the "hash of its content" part of this last point means that as long as 2 different pieces of content are identical (ie. file contents are identical), the resulting hash will also be identical. This also illustrates the fact that blobs are decoupled from a (file)name, as it has no bearing on the produced hash.

To get the id of a blob, git will take the content of the blob and add a prefix, then pass it into a SHA1 algorithm to compute the hash.
- the prefix is of the format `<type> <length>\0 <content>"
	- ex. `blob 11\0my content`
	- git does this prefixing for us automatically when we call `git hash-object`
- This demonstrates how each object is identified with its hash, and the type of the object is baked into that hash

If we write 2 different files with the same content to the object database, only one SHA will get recorded, since the same content hashed will always produce the same result.
- the same can be said for a tree object, but not for a commit object, since the author and time of commit makes their content always unique.

The object is created as soon as it is known to git (ie. when added to index)

