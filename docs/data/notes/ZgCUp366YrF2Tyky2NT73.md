
Torrent is the usual shorthand for BitTorrent

To download something via BitTorrent is to download parts of the file from various peer nodes in the network. Once all parts of the file are received, they can be reassembled to form the full file.

BitTorrent is a communications protocol for P2P sharing (which is by nature decentralized)

Each node in the BitTorrent network is a BitTorrent client which implements the protocol.

### Terminology

SEEDERS are those who has downloaded the file already or initially only one person who uploads the torrent seeds to others. You may notice that after your download is complete the torrent turns from DOWNLOADING to SEEDING.

PEERS are those who are downloading and uploading at the same time. They do not possess the whole file. They only possess parts of whole.

LEECHERS are those who don’t have all parts of the file and thus are not able to share with you the required part of the file.

### Example: downloading the alphabet
Imagine you're joining the English alphabet, but the letter G is missing (only the original seed, The Library, has a copy of it). So you'll get A-F and H-Z (99% completion), as will anybody else joining - you pass them around until everybody has a copy of everybody else's letters. Eventually you all have the same set, 400 of you all missing G.

Then one year later, The Library rejoins the swarm. By this point, 350 of the original peers have gotten bored and removed the incomplete torrent. The 50 who kept going now get the missing G and have 100% compete copies of the alphabet. Hooray! 25 of them disconnect, as does The Library, but now there are 25 seeds remaining in case any new peers join the swarm. So long as at least one of them stays online, future peers can still get 100% of the file.

### Tracker
BitTorrent trackers provide a list of files available for transfer and allow the client to find peer users, known as "seeds", who may transfer the files.

A BitTorrent tracker is a special type of server that assists in the communication between peers using the BitTorrent protocol.

The tracker server keeps track of where file copies reside on peer machines (in other words, where the file that other torrent users are trying to download are)

Connecting to a tracker is no longer needed after the initial peer-to-peer file download is started

### Magnet Link
A magnet link is a type of hyperlink that enables the downloading of files and data from [[P2P|network.internet.p2p]] sharing networks, particularly torrent networks. It works in a server-less environment and contains all the information a torrent client requires to download a specific file. Once the user clicks a magnet link, its data is sent to the desktop torrent client software, which automatically starts the download.
- It is preferable to a .torrent file because it eliminates the need to download a tracker file and search for uploading peers.
- Therefore, a magnet link replaces a .torrent file extension/mechanism with only a hyperlink, which consists of the magnet identifier, file name and cryptographic content hash.

The idea of magnets is instead of downloading the `.torrent` file from a webserver, you download it directly from a seed/leecher. The biggest advantage is that you might be able to download the content of the torrent, even if the tracker is down or closed for registration.

The main advantage for Bittorrent indexers (e.g. PirateBay) is that they do not have to store the torrents on their servers anymore which could be beneficial for them in several ways. It could reduce the pressure from the media creation industry and reduce hardware infrastructure expenses thanks to less tracking and downloading.

## Resources
- See the IP address used when downloading a torrent: https://torguard.net/checkmytorrentipaddress.php
    - right-click on the Green banner and copy the link (it’s a Magnet link) Then add this link into your torrent client and start the torrent. After a few seconds, the site will show the IP address of the connection it finds.
- VPNs that support Port Forwarding
    - https://www.reddit.com/r/VPNTorrents/comments/s9f36q/list_of_vpns_that_allow_portforwarding_2022/