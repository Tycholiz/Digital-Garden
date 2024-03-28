
### Limitations of Websockets
- web components such as firewalls and load balancers are built and maintained around HTTP

* * *

### Websockets vs Server Sent Events (SSE)
Websockets and SSE are both capable of pushing data to browsers, however they are not competing technologies. 
- Websockets connections can both send data to the browser and receive data from the browser. 
    - A good example of an application that could use websockets is a chat application.
- SSE connections can only push data to the browser. Online stock quotes, or Twitter's updating timeline or feed are good examples of an application that could benefit from SSE.

In practice since everything that can be done with SSE can also be done with Websockets, Websockets is getting a lot more attention and love, and many more browsers support Websockets than SSE. However, it can be overkill for some types of application, and the backend could be easier to implement with a protocol such as SSE.

#### Caveats
- SSE suffers from a limitation to the maximum number of open connections, which can be specially painful when opening various tabs as the limit is per browser and set to a very low number (6).
- Only WS can transmit both binary data and UTF-8, SSE is limited to UTF-8.