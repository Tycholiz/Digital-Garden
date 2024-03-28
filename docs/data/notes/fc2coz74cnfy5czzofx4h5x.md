
### Storage Pools
Think of a storage pools as a group of your HDDs that you decide to group

Storage pools are mostly about RAID levels. If you want one RAID for all disc - use one pool for all of them. But maybe you want to have large HDD for storage and fast SSD for docker? Obviously you don't want to put HDD and SSD in one RAID. So, you'll have two storage pools.

you cannot reduce an existing Storage Pool's number of drives or replace its drives with smaller capacity ones, since data are distributed across the drives in most RAID types
- if you must, follow the [guide](https://kb.synology.com/en-in/DSM/tutorial/Reduce_RAID_drives) to do so

### Volumes
A volume is a collection of HDDs (or even just one)

Volumes provide the basic storage space on your Synology NAS. All of your stuff—such as shared folders, documents, and package data—is stored on volumes. Therefore, before you start storing anything on your Synology NAS, you will need to create at least one volume.

Think of a volume as a Partition, if you think of the RAID group as one HDD.

Your volume is assigned to a Storage Pool and does not know what is the underlying hardware structure of that pool.

mostly about filesystem: ext4 vs btrfs. 
- Unless you have specific reason and you know what you're doing, - just use btrfs and do one volume per storage pool.

You may want to create multiple volumes if you previously created multiple pools with the same or different type of RAID and/or disk numbers

#### Example usage
- volume 1 is 6 bays and holds my data, movies, personal photos etc
- volume 2 is a single drive that records surveillance and therefore is very heavy use. I don't want all my disks working that hard. I figure it would be a slim coincidence for it to go when I do need to review its data.
- volume 3 is a single SSD for my dockers including plex config for speed purposes. backup up daily, so no big deal if I lose a day.

### Shares
Folders inside the Volume.
- Under any volume you will create "Shares" which are actually folders and this is how you manage your file tree.

### Shared Folder
A Shared Folder is the basic directory for you to store files and folders on your Synology NAS. 
- Therefore, you need to create at least one shared folder before you can start storing data.

### LUN (Logical Unit Number)
A LUN is loosley the equivalent of a Partition


* * *

### `homes/` and `home/` directories
Each user has their own `home/` directory that lives within the `homes/` directory
- `homes/` is owned by admin
- home folder should only be used for personal files and not for sharing with others.