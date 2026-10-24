
Volumes are the primary way to persist data that exists in a Container
- in a node app, we specify the following `volume` field attributes
```yml
volumes:
- '.:/app'
- '/app/node_modules'
```
- This first line is saying "map the host's PWD (.) to the `/app` directory in the container". The next line says "persist the node-modules directory in the container so that when we mount the host directory (the first line) at runtime, the node-modules won't get overwritten"
	- In other words, this would happen:
		- Build - The node_modules directory is created in the image.
		- Run - The current directory is mounted into the container, overwriting the node_modules that were installed during the build.

Volumes are managed by Docker and are best suited for persisting data that should persist beyond the lifecycle of any individual container. They offer more flexibility in terms of storage drivers and management.

## Bind Mount
When using a bind mount, a file/directory on the host machine is mounted into a container
- This is in contrast to using volumes, which involves Docker creating a directory on the host that a container can reach out and have full access to
- A bind mount lets us create something similar to a symbolic linked directory, whereby a directory or file on the host machine gets mounted into the container.
	- This means that we can make changes to the directory/file on the host machine, and those changes will reflected in the docker container's version of the directory/file. 
	- On the other hand, when you use a volume, a new directory is created on the host machine, which is created within Docker's storage directory (which is managed by Docker)
- bind mounts are created in `docker-compose.yml` with the `volumes` directive
	- unlike with `volumes`, we must specify the path of our host machine
- Bind mounts have limited functionality compared to volumes.
	- Bind mounts are more straightforward and directly link a directory or file from the host to the container. They are suitable for development and local testing, but they lack some of the features of volumes, such as management and portability across different environments.

![](/assets/images/2024-03-16-17-39-07.png)

# UE Resources
- [Primer](https://docs.docker.com/storage/volumes/)
