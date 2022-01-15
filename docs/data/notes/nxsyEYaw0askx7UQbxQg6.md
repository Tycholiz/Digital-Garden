
## Volumes
- The primary way to persist data that exists in a Container
- in a node app, we specify the following `volume` field attributes
```
volumes:
      - '.:/app'
      - '/app/node_modules'
```
- This first line is saying "map the host's PWD (.) to the `/app` directory in the container". The next line says "persist the node-modules directory in the container so that when we mount the host directory (the first line) at runtime, the node-modules won't get overwritten"
	- In other words, this would happen:
		- Build - The node_modules directory is created in the image.
		- Run - The current directory is mounted into the container, overwriting the node_modules that were installed during the build.

## Bind Mount
- Instead of Docker creating a directory on the host that a container can have full access to (volume), we can make use of bind mounts. A bind mount lets us create something similar to a symbolic linked directory, whereby a directory or file on the host machine gets mounted into the container.
	- This means that we can make changes to the directory/file on the host machine, and those changes will reflected in the docker container's version of the directory/file. 
- bind mounts are created in `docker-compose.yml` with the `volumes` directive

# UE Resources
[Primer](https://docs.docker.com/storage/volumes/)
