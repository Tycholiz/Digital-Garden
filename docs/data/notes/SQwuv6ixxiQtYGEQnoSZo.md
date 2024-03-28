
## Overview
an image is an immutable snapshot of a system
- The fact that it is immutable gives it predictability. In other words, it will always work as-is

## Under the hood
an image has a tree hierarchy. There is a base image (aka Parent Image), which is initiated with the `FROM` command in the Dockerfile. It sets the base for the rest of the images generated in the Dockerfile.
- Therefore, every Dockerfile must have a `FROM` directive

## Layers
Docker image is made up of a series of read-only `layers`
- Each line in the `dockerfile` creates a `layer`
	- Therefore, a layer contains only the differences between the preceding layer and the current layer.
- On top of the layers, there is a writable layer (the current one) which is called the container layer
- layers may be shared between images
	- this means if the layer `COPY . /app` is used in multiple places, each iteration of it (from other Dockerfiles) will not contribute to Docker's overall footprint.
	- When the Dockerfile is run with `docker build`, each layer gets executed and its result cached
- The following Dockerfile instructions create a layer and influence the size of the image:
	- `RUN`
	- `COPY`
	- `ADD`
- The other Dockerfile instructions create intermediate layers, and do not increase the size of the image
- When we build an image from Dockerfile, we'll notice that it says `removing intermediate container`, rather than what we might expect: `removing intermediate layer`. This is because a build step (ie. a line in the Dockerfile) is executed in an intermediate container, which is no longer needed once the build step is done
- if we run `docker history <image-id>`, we can recognize the intermediate containers as the ones having 0B size
	- There are also a lot of containers labelled `missing`, meaning that those layers are built on a different system and are not available locally.

```dockerfile
# create a layer from the ubuntu:15.04 image. 
FROM ubuntu:15.04
# add some files from your Docker client’s current directory. 
COPY . /app
# build your application using the make command. 
RUN make /app
# specifies what command to run within the container.
CMD python /app/app.py
```
In the previous example, we spun up a container whose basis is a Ubuntu server. We can just as easily spin up a continer whose basis is nodejs.

## Image Hierarchy
The image you inspect may have one or more base images. This means the author of the image used other images as starting points when building the image.
- Often these base images are either OS images such as Debian or Ubuntu, or programming language images such as Python, Node or Java.

## Tags
- an alias to the ID of an image. ie. they are just a a way to refer to a specific image
- anal: git tags can be used to refer to a specific commit (ex. map tag SHA 3fhd883nnf9 to v1.4)

## Architecture
Images are built to run on particular platforms. Therefore, their architectures must be specified while being built (e.g. `arm64`, `amd64`)
`docker buildx build --platform linux/amd64 -t neverforget-server:0.0.4 .`