
# K Anonymity
- When entering a password, the client hashes it into a 130 bit hash, then sends only the first several characters to the server. The server checks its database of passwords hashes, and returns all of the passwords that begin in the same way. Once the client receives this, it selects the hash that matches with its password
	- anal: this is similar to when you are authenticating in a government website, and it asks you questions like: "which of the following 4 addresses did you live at in 2016?"
- The benefit of this is that a password hash is never actually sent from the client to the server. MITM attacks are less threatening. The server also doesn't gain any valuable information, since it won't know if any of the hashes it is sending actually matches the one owned by the client.
- ex. run `curl https://api.pwnedpasswords.com/range/f42b7e
`. We will get in return a list of hashes for passwords that have been cracked (ie. leaked in plain white text)
