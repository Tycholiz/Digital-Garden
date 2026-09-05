
Soft symlinking a directory will not link the contents as well;

A symbolic or soft link is an actual link to the original file, whereas a hard link is a mirror copy of the original file. If you delete the original file, the soft link has no value, because it points to a non-existent file.

But in the case of hard link, it is entirely opposite. Even if you delete the original file, the hard link will still have the data of the original file, because hard links act as a mirror copy of the original file.

In a nutshell, a soft link...
- can cross the file system,
- allows you to link between directories,
- has different inode number and file permissions than original file,
- permissions will not be updated,
- has only the path of the original file, not the contents.
A soft link is something like a shortcut in Windows. It is an indirect pointer to a file or directory. Unlike a hard link, a symbolic link can point to a file or a directory on a different filesystem or partition.

A hard Link...
- can't cross the file system boundaries (i.e. A hardlink can only work on the same filesystem),
- can't link directories,
- has the same inode number and permissions of original file,
- permissions will be updated if we change the permissions of source file,
- has the actual contents of original file, so that you still can view the contents, even if the original file moved or removed.
You can think a hard link as an additional name for an existing file. Hard links are associating two or more file names with the same inode . You can create one or more hard links for a single file. Hard links cannot be created for directories and files on a different filesystem or partition.
