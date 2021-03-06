
## Commands
### run a command in a new container
`docker run`
- spec: Each time we execute `docker run`, the Dockerfile is read. This is why calling `docker run` on an existing container wouldn't make any sense.

### show all running containers in docker engine
`docker ps`
- pass `-a` to see all containers
- `docker-compose ps` will list containers in the docker engine that are related to the images specified in `docker-compose.yml` (therefore `dc ps` is a subset of `docker ps`)

## Start a container
`docker container run --publish 8000:8080 --detach --name bb <APP NAME>`
- `--publish` asks Docker to forward traffic incoming on the host’s port 8000, to the container’s port 8080
  - containers have their own set of ports
- notice, we didn’t specify what process we wanted our container to run. We didn’t have to, since we used the CMD directive when building our Dockerfile; thanks to this, Docker knows to automatically run the process npm start inside our container when it starts up
- visit application at localhost:8000

### Show all containers
`docker container ls`

#### Filter containers by those created by *mongo* image
`docker container ls -f ancestor=mongo`
