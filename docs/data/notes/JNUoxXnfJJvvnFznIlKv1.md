
# Adb reverse and port forwarding
`adb -s 192.168.76.101:5555 reverse tcp:8081 tcp:8081`
In either case, it's basically just port forwarding. It's probably called "reverse" because it's actually setting up a [[reverse proxy|webserver.proxy.reverse]] in which a http server running on your phone accepts connections on a port and wires them to your computer... Or vice versa. The port is specified twice because you have the ability to control the listening port on both sides of the proxy.

When the RN packager is running, there is an active web server accessible in your browser at 127.0.0.1:8081. It's from this server that the JS bundle for your application is served and refreshed as you make changes. Without the reverse proxy, your phone wouldn't be able to connect to that address.

Now when your phone tries to access `http://localhost:3000` your request will be routed to localhost:3000 of your laptop. Just recompile your app to use localhost:3000 as the API endpoint.

if we had written `adb reverse tcp:80 tcp:3000`, it would redirect your phone's port 80 to your computer's port 3000
