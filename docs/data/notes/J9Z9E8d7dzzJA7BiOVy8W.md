
A tree is Git's representation of snapshots, meaning they store the state of a directory at a given point (without notion of time/author). 
- To tie trees together into a coherent history, Git wraps each one in a commit object and specifies a parent commit. By following the parent of each commit, we can walk through the entire history of the project.
- Each commit refers to only one tree object.

A tree can contain many trees and many blobs.

Multiple trees may point to the same blob
- this happens when a file's contents (ie the blob) do not change between a commit.
	- therefore, most blobs are referenced by many trees.

![](/assets/images/2022-04-08-22-01-55.png)

The Tree holds pointers to filenames and other trees, effectively allowing us to group files together (which is essentially what a directory is)
- A tree object contains 1+ entries. Each of which is either a pointer to a blob or a pointer to a subtree.

![](/assets/images/2022-04-08-22-01-00.png)

The tree object is what associates the filename (or directory name) with its content.
- We can confirm this by running `git cat-file` on the tree object. It will give us back a list of blobs and their associated filenames
- We can use a plumbing command `update-index`, which effectively allows us to associate an existing blob with a filename:
	- `git update-index --add --cacheinfo 100644 83baae61804e65cc73a7201a7252750c76066a30 test.txt`
		- add a file to the index (`--add`), get it from the object database (`--cacheinfo`)
		- upon executing this command, we have `test.txt` added to the staging area.
		- `100644` represents the file permissions on disk, showing that trees determine Unix file permissions for each blob
			- only cares about `100644` (normal file), `100755` (executable), `120000` (symbolic link)

The tree is normally made by examining the state of the staging area

The tree itself doesn’t know where it exists within the repository, that is the role of the objects pointing to the tree. The tree referenced by `<ref>^{tree}` is a special tree: the root tree. This designation is based on a special link from your commits.

The fact that git doesn't really about folder names is precicely the reason why you cannot commit an empty directory. Often on Github you will notice that people put a blank `.keep` file. This naming is meaningless to Git, but it is just a convention, as if to say "hey, I just put a placeholder here so we can still get the empty directory to be part of our tree."
