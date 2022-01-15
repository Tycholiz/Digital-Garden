
## HTTP Requests

#### Post
```
curl -X POST [options] [URL]
```

example
```sh
curl -X POST -H "Content-Type: application/json" \
    -d '{"email": "linuxize@example.com", "password": "123"}' \
    http://localhost:5678/login
```

#### Put
```
curl -X PUT -d arg=val -d arg2=val2 localhost:8080
```

#### Setting return value of curl to variable
```
http_code=$(curl -s -o /dev/null -w "%{http_code}" https://www.google.com)
```
