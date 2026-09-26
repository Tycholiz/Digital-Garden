
Long polling is a technique where the server elects to hold a client’s connection open for as long as possible, delivering a response only after data becomes available or a timeout threshold has been reached.
- Long polling is essentially a more efficient form of the original polling technique. The difference is that we don't need to make repeated requests.

Implementing long polling is a server-side concern, meaning that if it is a third party application we plan to long poll, they must have built the functionality in.

The only difference to basic polling, as far as the client is concerned, is that a client performing basic polling may deliberately leave a small time window between each request so as to reduce its load on the server, and it may respond to timeouts with different assumptions than it would for a server that does not support long polling.
- With long polling, the client may be configured to allow for a longer timeout period (via a Keep-Alive header) when listening for a response – something that would usually be avoided seeing as the timeout period is generally used to indicate problems communicating with the server.

Long polling existed as the conventional technique to implementing (pseudo) real-time communication prior to the advent of WebSockets.
