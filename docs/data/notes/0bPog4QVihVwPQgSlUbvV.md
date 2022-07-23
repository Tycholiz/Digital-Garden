
The user interacts with FTP via an FTP user agent.
- once the hostname is provided by the client, the FTP client process establishes a TCP connection with the FTP server.
    - after this, the user provides username/password to establish the connection.

One key difference between HTTP and FTP is that FTP uses two parallel TCP connections to transfer a file: a control connection, and a data connection.
- *control connection* is for passing control information between the 2 hosts, such as username, password, commands to change remote directory, commands to get/put files etc.
- *data connection* is for actually sending files back and forth.

Unlike HTTP, FTP is stateful; the server maintains state about the FTP client, most importantly associating the connection with a particular user.
- also it must maintain state about the user's present working directory of the server.
- keeping this state significantly limits the total number of sessions that an FTP server can handle.

FTP connections happen on port `21`
