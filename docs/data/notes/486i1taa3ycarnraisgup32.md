
The simple way to handle authentication is by passing a basic authorization header:
```
Authorization: Basic am9lc2htb2U6MTIzNDU=
```

Where the value is `username:password` [[base64|binary.encoding.base64]] encoded.

This puts an unnecessary strain however on the database server, since it needs to compute the hash with each request.

One database per user is the general recommended approach for storage in CouchDB. Therefore, each user would store their own couchdb login credentials.
- https://docs.couchdb.org/en/3.2.0/config/couch-peruser.html

## Security
Security in a single CouchDB can only be set up to do either:
- Everyone can read/write everything (admin party)
- Everyone can read, some can write
- Some can read everything, and those same people can write everything