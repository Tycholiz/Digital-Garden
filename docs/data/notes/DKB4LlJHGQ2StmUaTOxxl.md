
a component of the kernel
- handles all system calls related to files and file systems
- serves as an interface between a user and a particular file system
	- In other words, it abstracts away the specific filesystem implementation and let's us access it on a command line.
	- it accomplishes this by specifying an interface (a contract) between the kernel and the underlying FS
- we interact with the underlying FS by using the API provided by the VFS. 
- this abstraction allows us to bridge the differences between Windows filesystems, Mac filesystems, and Unix filesystems
- when an external device attached to the system (such as a USB stick), Unix can run the `mount` command, which will create a new directory on the VFS.
