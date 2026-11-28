
We need to load the proxy and proxy_http modules in Apache, and the most basic configuration would look like this (for one domain):

```xml
<VirtualHost *:80>
    ServerName foo.example.com
    ProxyPass / http://192.168.2.x
</VirtualHost>
```
