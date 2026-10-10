
There has to be a clear distinction between your different layers of storage. For instance, you may have storage to sync config settings, warm storage for things that you want to be able to retrieve relatively easily (photos 3 years ago), cold storage for bulletproof backups (ex. all your pictures ever taken).

Making this distinction clear helps us realize if our current backup methods are good enough. For instance, as in the first example, what if we are using Dropbox to sync our dot-files across multiple devices. Since this file is actively used, it is no longer stable as a backup solution. All it takes is for one device to delete all of the contents on the folder for your entire "backup solution" to fall over. Therefore, we need another solution. Because dot-files get updated often, we will need to write to this often. Therefore, a warm storage solution is probably sufficient. Following the rule of 3-2-1,

If something is built into a software workflow, then it is not a backup
- ex. Joplin sync does not count as backup, nor does Mackup. The reason is that we interact with these tools in a dynmaic way. We don't treat them as immutable snapshots. A backup is only a backup if it's immutable (or treated as such).

If you haven't yet tried to restore all of your data via your backup file, then you don't have a backup system in place.

#### 3-2-1 rule of backups
Follow the 3-2-1 rule of backups - 3 copies, on 2 different media (e.g. disk, usb), with at least one copy offsite.
