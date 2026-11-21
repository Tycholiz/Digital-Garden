
a filesystem that is distributed across a network
- distributed means that the FS does not share block level access to the data, instead opting for a network protocol (likely IP)

- the NFS mounts directly into the file system on mount points, so we can use regular unix commands like `cp`, `ls` etc.
- ex. a NAS exposes its data via a NFS
