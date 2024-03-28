
ext4 is a journaling file system for Linux
- A journaling file system is a file system that keeps track of changes not yet committed to the file system's main part by recording the goal of such changes in a data structure known as a "journal", which is usually a circular log. In the event of a system crash or power failure, such file systems can be brought back online more quickly with a lower likelihood of becoming corrupted
- For example, deleting a file on a Unix file system involves three steps:
    1. Removing its directory entry.
    2. Releasing the inode to the pool of free inodes.
    3. Returning all disk blocks to the pool of free disk blocks.

The ext4 filesystem can support volumes with sizes in theory up to 64 zebibyte (ZiB) and single files with sizes up to 16 tebibytes (TiB) with the standard 4 KiB block size, and volumes with sizes up to 1 yobibyte (YiB) with 64 KiB clusters, though a limitation in the extent format makes 1 exbibyte (EiB) the practical limit.

ext4 uses checksums in the journal to improve reliability, since the journal is one of the most used files of the disk.