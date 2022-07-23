
Docker Compose is a tool for defining and running multi-container applications
- use `docker-compose.yml` to configure the app's services, and start them with a single command
	- this file will be parsed everytime you run `docker-compose up`. Therefore, if you make changes to `docker-compose.yml`, all you need to do is `docker-compose down`, then `dc up` again. This is unlike the Dockerfile, which will only be run once when you are building an image

using compose is a 3 step process:
1. Define your app’s environment with a Dockerfile so it can be reproduced anywhere.
2. Define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment.
3. Run docker-compose up and Compose starts and runs your entire app.

Compose has commands for managing the whole lifecycle of your application:
- Start, stop, and rebuild services
- View the status of running services
- Stream the log output of running services
- Run a one-off command on a service

## Anatomy of docker-compose.yml
This file will be run with `docker-compose up`, building up the services (and with it, the containers) of the whole application
- if the image has already been built, then by default the build command will be ignored

The docker-compose.yml file is organized by service. The `build` field designates the location of the service's dockerfile (this location is called the "build context") so that it can be run and the image can be built.

the docker-compose.yml file can specify environment variables to be executed during process of building the images with the `args` field under `build`
- these variables are accessed in the dockerfiles by specifying `arg`:

dockerfile:
```yml
arg buildno
arg gitcommithash

run echo "build number: $buildno"
run echo "based on commit: $gitcommithash"
```

docker-compose.yml
```yml
build:
  context: .
  args:
    buildno: 1
    gitcommithash: cdc3b19
```

### Publish vs. EXPOSE
- these commands make the services inside the containers available to outside the containers
- expose makes the processes in the container accessible to other containers within Docker (but not from *outside* Docker)
	- note: this is not entirely correct, but useful for understanding. [see](https://stackoverflow.com/questions/22111060/what-is-the-difference-between-expose-and-publish-in-docker/47594352#47594352)
- publish (`-p`) makes the services available to outside Docker
	- Therefore, publish implicitly EXPOSES

## Service
- there is one image per service, but there can be multiple containers for a service. In other words, a docker "service" is one or more containers from one image
- In our development set up, we are likely to have a single container per image. When we start to scale and need for additional containers per service arises, we can use `docker service` to create multiple containers from the same image
	- consider how this relates to the `docker run` command, which starts up 1+ containers from an equal amount of image (ie. one container per image)
	- what arises from running this command is Docker's swarm mode
- Swarm mode is a container orchestration tool, giving the user the ability to manage multiple containers deployed across many different host machines
	- ex. imagine there are 5 containers deplayed across 5 different machines across USA. Swarm mode allows you to interact with all 5 containers at once, instead of having to manage one container at a time
- This above distinction between a container and a service is precicely what differentiates `docker exec` and `docker-compose run`
	- naturally, `docker-compose` runs `docker` commands under the hood.
	- `docker exec` exectutes a command within a docker container
	- `docker-compose run` executes a command on a service (with the help of a swarm manager(?)

# UE Resources
[Definitive Guide to Docker Compose](https://gabrieltanner.org/blog/docker-compose)
