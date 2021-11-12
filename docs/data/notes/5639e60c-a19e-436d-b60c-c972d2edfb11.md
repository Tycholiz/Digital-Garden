
# Edge Server
- def - a server that acts as a gateway so that one network can access another
	- In other words, the edge server enables the two networks to communicate.
- Therefore, the ability for clients to make a network connection to other clients is bottlenecked by the number of edge servers between the two clients;
- Generally speaking, the farther the connection must travel, the greater the number of networks that must be traversed.
![](/assets/images/2021-03-11-15-51-29.png)
- "edge" refers to the philosophy of geographically placing the data close to the server (or proxy server) that requests it 
- Edge servers are contrasted with Origin Servers, which is your actual web server (ex. Express).
- spec: caches want to live as close as possible to the client, while edge servers tend to live further out, closer to the internet (but still closer to the client than the origin server)
