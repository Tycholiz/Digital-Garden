
## Overview
an image is an immutable snapshot of a system
- The fact that it is immutable gives it predictability. In other words, it will always work as-is

## Under the hood
an image has a tree hierarchy. There is a base image (aka Parent Image), which is initiated with the `FROM` command in the Dockerfile. It sets the base for the rest of the images generated in the Dockerfile.
- Therefore, every Dockerfile must have a `FROM` directive

### Layers
- Docker image is made up of a series of `layers`
- Each line in the `dockerfile` creates a `layer`
	- Therefore, a layer contains only the differences between the preceding layer and the current layer (like consequent git commits).
- layers are read-only
- On top of the layers, there is a writable layer (the current one) which is called the container layer
- layers may be shared between images
	- this means if the layer `COPY . /app` is used in multiple places, each iteration of it (from other Dockerfiles) will not contribute to Docker's overall footprint.
	- When the Dockerfile is run with `docker build`, each layer gets executed and its result cached
- The following Dockerfile instructions create a layer and influence the size of the image:
	- `RUN`
	- `COPY`
	- `ADD`
- The other Dockerfile instructions create intermediate layers, and do not increase the size of the image
- When we build an image from Dockerfile, we'll notice that it says `removing intermediate container`, rather than what we might expect: `removing intermediate layer`. This is because a build step (a line in the Dockerfile) is executed in an intermediate container, which is no longer needed once the build step is done
- if we run `docker history <image-id>`, we can recognize the intermediate containers as the ones having 0B size
	- There are also a lot of containers labelled `missing`, meaning that those layers are built on a different system and are not available locally.

Below, The FROM statement starts out by creating a layer from the ubuntu:15.04 image. The COPY command adds some files from your Docker client’s current directory. The RUN command builds your application using the make command. Finally, the last layer specifies what command to run within the container.
```dockerfile
FROM ubuntu:15.04
COPY . /app
RUN make /app
CMD python /app/app.py
```
In the previous example, we spun up a container whose basis is a Ubuntu server. We can just as easily spin up a continer whose basis is nodejs.

## Dockerfile
- Think of these Dockerfile commands as a step-by-step recipe on how to build up our image.
	- first step to containerizing an application
- Dockerfiles describe how to assemble a private filesystem for a container, and can also contain some metadata describing how to run a container based on this image
	- A Dockerfile specifies the operating system that will underlie the container, along with the languages, environment variables, file locations, network ports, and other components it needs— and, of course, what the container will actually be doing once we run it.

Example Dockerfile
```dockerfile
FROM node:6.11.5
WORKDIR /usr/src/app
COPY package.json .
RUN npm install
COPY . .
CMD [ "npm", "start" ]
```

Building up our images takes the following steps:

1. Start FROM the pre-existing node:6.11.5 image. This is an official image, built by the node.js vendors and validated by Docker to be a high-quality image containing the node 6.11.5 interpreter and basic dependencies.
2. Use WORKDIR to specify that all subsequent actions should be taken from the directory /usr/src/app in your image filesystem (in other words, not the FS on your machine).
3. COPY the file package.json from your host to the working directory in your image (so in this case, to /usr/src/app/package.json)
4. RUN the command npm install inside your image filesystem (which will read package.json to determine your app’s node dependencies, and install them)
5. COPY in the rest of your app’s source code from your host to your image filesystem.

These above commands effectively set up the filesystem of our image

CMD specifies how to run a container based off *this* particular image
- In this case, it’s saying that the containerized process that this image is meant to support is npm start.
- i.e. it is a metadata specification
- there can only be one `CMD` instruction per Dockerfile

ENTRYPOINT allows us to configure the container to run as an executable
- the commands specified in ENTRYPOINT will always be run.
- we also have CMD, whose commands will only run if we are spinning up a container and not explicitly setting any CLI arguments
	- if we specify arguments when spinning up a container, CMD is ignored, but ENTRYPOINT commands will still be executed
	- ex. `docker run -it <image> <arg1>`
	- CMD therefore are default arguments

## Tags
- an alias to the ID of an image. ie. they are just a a way to refer to a specific image
- anal: git tags can be used to refer to a specific commit (ex. map tag SHA 3fhd883nnf9 to v1.4)
