
# Unix filesystem
The UNIX filesystem unifies all physical hard drives and partitions into a single directory structure. It all starts at the top–the root (/) directory. All other directories and their subdirectories are located under the single Linux root directory. This means that there is only one single directory tree in which to search for files and programs.

## Types of files
- ordinary file
- directory
- special (device) file - represents a physical device (ex. printer).
	- special files are used for sending outputs to the device, and receiving inputs
- pipe - a temporary file that gets created when we pipe commands into each other. Pipes are the mechanism through with programs chain together.
- socket - a stream of data very similar to network stream, but all the transactions are local to the filesystem.
- symbolic link

## Filesystem metadata
- The general file system model, to which any implemented file system needs to be reduced, consists of several well-defined metadata entities: `superblock`, `inode`, `file`, and `dentry`
	- they are metadata about the filesystem
- each entity here interacts using the VFS, and each entity is treated as an object.
- each entity has its own data structure, and a pointer to a table of methods 

## Mount Point
- a mount point is a special directory within the unix filesystem. It is special because when an external filesystem is mounted (ex. HDD, SSD, USB, SD), its contents are dumped into the mount point, which is accessible from the root folder (`/`)
- ex. `/media`
- so when we run `mount /dev/hda2 /media/photos`, we are saying "hey, I want you take all of the contents of `/dev/hda2` (partition 2 of HDD), and make it all accessible through `/media/photos`".
	- spec:think of it like a map, where it will take the external file system on the USB (in FAT or NTFS format) and it will make all of the contents visible within the UNIX filesystem
	- to carry out this "mapping", unix has filesystem drivers 

## Directory structure
`/bin` - binaries generally needed by all users of the system.
`/lib` - contains system libraries
`/usr` - contains executables, libraries, and shared resources that are not system critical (ex. X Window)
`/usr/bin` - stores binaries distributed with the OS, that aren't in `/bin` 
`/usr/lib` - stores libraries for programs in `/usr`
`/dev` - contains file representations of peripheral devices. Therefore, to view files, the external device needs to be mounted (see Mount Point below) 
`/media` - default mount point for removable devices
`/mnt` - default mount point for temporary filesystems
`/etc` - contains system-wide config files/system databases.
`/proc` - contains all running processes displayed as their own files.
`/tmp` - files that get cleaned frequently. Often, this directory gets cleared on reboot. 
`/var` - contains files that get changed often. usually where files go that are not managed by the system-wide package manager
`/var/mail` - contains all incoming mail
`/var/www/html` - default root folder of the web server (e.g. Apache)

### Subdirectories
`sys/` - operating system kernel, handling memory management, process scheduling, system calls etc.
