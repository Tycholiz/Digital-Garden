
Packfiles can be thought of compressed [[blobs|git.plumbing.objects.blob]].

Since different versions of the same file create multiple blobs, it would be inefficient if the only difference between these versions was a single line. What would be great is if we could store one version of the file, and just store the delta (ie. diff) of the other with the first.
- Git does exactly this using packfiles.

The initial format in which Git saves objects on disk is called a "loose" object format. Occasionally Git packs up several of these objects into a single binary file called a *packfile* for the purpose of being more efficient.

Packfiles are created under 3 circumstances:
1. there are too many loose objects around (~7000)
2. `git gc` is run manually
3. we push to a remote repo 

When we run `git gc`, we will notice that many of the objects in our object database disappear and are replaced by packfiles. The objects that remain are those blobs that aren't pointed to by any commit (in other words, the blobs were never added to any commits). Because of this, those blobs are considered dangling and as a result were never packed up into the new packfile.
- The other files are the packfile and an index. The packfile contains the contents of all the objects that were removed from the filesystem. The index can be thought of as the index of a textbook, which helps us locate specific objects quickly.
	- if we inspect this packfile index with `git verify-pack`, we can see that 2 versions of the same file will have one version referencing the other, showing that we use deltas to be efficient. 
		