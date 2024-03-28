
tagging your image with the fully-qualified name of your registry allows us to tell Docker where to upload the image
```sh
docker tag neverforget-server:0.0.2 registry.digitalocean.com/neverforget/neverforget-server:0.0.2
```

After that, you can push it
```
docker push registry.digitalocean.com/neverforget/neverforget-server:0.0.2
```