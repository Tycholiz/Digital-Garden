
With usenet, you are downloading a file directly from a server. The speed of your download, assuming a good provider, will almost certainly be the maximum your ISP allows.

Another point to mention is that with torrents, access to certain files can be limited to the specific torrent sites you are using. With usenet, when a file gets uploaded to a group, every indexer that is indexing that group has access to the same file. Whether a particular file gets posted or not, and whether certain obfuscation can be read, is up to the automation process of the indexer.

Before you are able to download anything with Usenet, you will need 3 things:
1. Provider
2. NZB Indexer
3. A Usenet client

### Providers
The provider grabs the files you need from your search by granting you access to the Usenet servers that host the files.
- [List of providers](https://www.reddit.com/r/usenet/wiki/providers/)

- ex. SABnzbd

### Indexers
Think of these as your search engines. These are the sites that will tell you where all the files for whatever thing you are looking for are (e.g NZB files)
- ex. NZBGeek, DrunkenSlug, and NinjaCentral 

The indexer information gets input into Sonarr/Radarr

### Usenet client
Once the NZB file is obtained via the Indexer, it is loaded into the usenet client. The client then connects to the Usenet server, downloads the required files, and may also handle the decoding of files that are often split into multiple parts or encoded for transmission efficiency.

- ex. NZBGet

Provider information gets put into the download client

### NZB Files
Instead of browsing Usenet directly, users often rely on NZB files. An NZB file is a small XML format file that contains information about the files available for download on Usenet