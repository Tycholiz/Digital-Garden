
*Arrs refers to the combined programs of Lidarr, Prowlarr, Radarr, Readarr and Sonarr.
- these programs are designed to automatically grab, sort, organize, and monitor your Music, Movie, E-Book, or TV Show collections

## Radarr, Sonarr, Lidarr, Readarr
These are search tools

Radarr, Sonarr, Lidarr and Readarr allow us to search for movies, tv shows, music and ebooks.
- these programs talk to Prowlarr, which looks around for the content and returns the location so the download client (e.g. qBittorrent) can download it.

Their job is to perform searches and track indexers for desired things, then submit download jobs to a download client (ie. a torrent or Usenet client)

These search tools will be notified by the download client when the download is complete, and automatically name the files and move them to their configured location.

The search tools will monitor your download client's active downloads associated with that category name (e.g movies, tv, series, music). 
- This monitoring occurs via your download client's API.

When using torrents, the downloaded content will remain in the download folder so that it can be seeded. However, it is hardlinked to the media folder, so it does not take up any additional space.
- you can confirm that the files are indeed hard-linked by verifying that the inode number is the same for each file (`ls -i`)

## Prowlarr
Prowlarr crawls the internet and finds torrent files for us, negating our need to search for them manually

### Indexer
An index is simply a torrent website that will be searched.

Examples: ThePirateBay, KickassTorrents

## Overseerr
Overseer automates the accepting of requests, and feeds those into Radarr/Sonarr/Lidarr, which automates looking for those requests