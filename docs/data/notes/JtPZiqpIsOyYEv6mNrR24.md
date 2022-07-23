
## Storage Formats 
There are 3 main ways (formats) that data can be stored digitally: 
1. file storage 
2. block storage 
3. object storage

### File storage
- organizes and represents data as a hierarchy of files in folders
- ex. Unix, NAS
- Since a system must be the thing that implements a filesystem, data stored in this way is not easily portable 
	- ex. using Windows FS and trying to open it on Mac will prove difficult

### Block storage
- chunks data into arbitrarily organized, evenly sized blocks. Each block is given an ID, making it easily retrievable from storage.
	- it takes several blocks to make up one file.
- Data can be retrieved fast because it doesn't rely on a single path to find the data.
- the more data you need to store, the better off you’ll be with block storage.
- ex. Storage Area Network (SAN)

### Object storage
- manages data and links it to associated metadata.
