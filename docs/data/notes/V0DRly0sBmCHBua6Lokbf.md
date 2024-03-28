
A docker registry is a stateless, scalable server-side application that stores and allows us to distribute docker images
- In other words, it allows us storage for our images

- Think of it like github for docker images
    - So we have commands like `docker pull`, `docker push` etc.
- ex. *Docker Hub*
- Docker registry stores images as 2 parts, with a pointer between them:
    - image:tag
    - digest (i.e. the SHA id)
- Docker Hub is an available registry that is hosted for us 
