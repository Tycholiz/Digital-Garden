
To make an app truly offline first, we primarily need to do two things:
1. Any code and assets used should be available offline
2. Any data changes should be made locally first and then synced to the cloud.

### using Apollo and resolver to serve offline-first
- the local cache can be used to remember what queries resolved to. The resolver helps the cache remember which updates it made. Therefore if the app is offline, the cache will be able to remember updates that were made to the cache, so that up-to-date information can be returned 
![](/assets/images/2021-03-28-19-52-42.png
