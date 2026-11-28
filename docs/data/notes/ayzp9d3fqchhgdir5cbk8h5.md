
Think of Dockerfile commands as a step-by-step recipe on how to build up our image.
- it is the first step to containerizing an application

Dockerfiles describe how to assemble a private filesystem for a container, and can also contain some metadata describing how to run a container based on this image
- A Dockerfile specifies the operating system that will underlie the container, along with the languages, environment variables, file locations, network ports, and other components it needs— and, of course, what the container will actually be doing once we run it.

Example Dockerfile:
```dockerfile
FROM node:6.11.5
WORKDIR /usr/src/app
COPY package.json .
RUN npm install
COPY . .
CMD [ "npm", "start" ]
```

Building up our images takes the following steps:

1. Start FROM the pre-existing `node:6.11.5` image. This is an official image, built by the node.js vendors and validated by Docker to be a high-quality image containing the `node 6.11.5` interpreter and basic dependencies.
2. Use WORKDIR to specify that all subsequent actions (e.g. COPY, RUN) should be taken from the directory /usr/src/app in your image filesystem (in other words, not the FS on your machine).
3. COPY the file package.json from your host to the working directory in your image (so in this case, to /usr/src/app/package.json)
  - The fact that we run `npm install` before copying over "everything else" is significant here. If we had copied everything and then run `npm install`, each file change would cause Docker to run `npm install`. Instead, by copying only `package.json`+`package-lock.json`, `npm install` will only get run when there are changes in those 2 files.
4. RUN the command npm install inside your image filesystem (which will read package.json to determine your app’s node dependencies, and install them)
5. COPY in the rest of your app’s source code from your host to your image filesystem.

These above commands effectively set up the filesystem of our image

`CMD` specifies how to run a container based off *this* particular image
- In this case, it’s saying that the containerized process that this image is meant to support is npm start.
- i.e. it is a metadata specification
- there can only be one `CMD` instruction per Dockerfile

`ENTRYPOINT` allows us to configure the container to run as an executable
- the commands specified in `ENTRYPOINT` will always be run.
- we also have `CMD`, whose commands will only run if we are spinning up a container and not explicitly setting any CLI arguments
	- if we specify arguments when spinning up a container, `CMD` is ignored, but `ENTRYPOINT` commands will still be executed
	- ex. `docker run -it <image> <arg1>`
	- `CMD` therefore are default arguments
