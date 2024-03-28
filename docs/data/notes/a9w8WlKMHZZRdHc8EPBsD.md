
A Container is an instance of an [[Image|docker.image]]

Containers decouple the machine's OS from the application dependencies and the code.
- Each container had its own filesystem (provided by a Docker image)
- Image includes everything needed to run an application
    - inc. the application code, runtimes, dependencies etc
- Containers are designed to be transient and temporary, but they can be stopped and restarted, which launches the container into the same state as when it was stopped.
- Containers can communicate with each other by attaching (`docker attach`). They do this by attaching stdin, stdout and stderr steams to one another so that one container's output can be piped into another container as their input 
	- When we run `docker attach` with a specified container, we are piping our stdin/stdout/stderr to the container, so we are effectively able to write commands in the container's terminal

containers can be created from any point in an image’s history, much like source control.

## From the container's perspective 
The container by default has no access to the outside world. When the docker host specifies the `-p` option when spinning up a new container, the container opens up the specified ports and maps them to the docker host's port.
- For all the container knows, it is a regular computer. It has a network interface, including an IP address, a gateway, a routing table etc. 

![9ede10f99d18b464b0087150a5679308.png](:/26150f69cbf24c1583bf667d69c6ac9b)

- Containers guarantee our applications will run the same anywhere, whether it's our own machine, a data centre, or anything else

## Run container
`docker run --name *<NAME OF CONTAINER>* -d -v /tmp/mongodb:/data/db -p 27017:27017 *<NAME OF IMAGE:IMAGE TAG>*`
- `--name`: Name of the container.
- `-v`: Attach the /tmp/mongodb volume of the host system to /data/db volume of the container.
- `-p`: Map the host port to the container port.
Last argument is the name/id of the image.

## Running commands in the container
- we can use `docker exec` to run commands.
- by default, the command is executed in the WORKDIR variable that is declared in the Dockerfile
- often, we see `docker exec -it <containerID> /bin/bash`
	- this executes the command `/bin/bash` within the specified container, opening a new bash session for us.
	- `-t` tells docker to open a terminal session
	- `-i` for interactive, ensures that our stdin input stream remains open 

## Stopping container
- `docker stop`: Send SIGTERM(termination signal), and if needed SIGKILL(kill signal)
	- use when you wish to clear up memory or delete all of the processes' -cached- data. Simply put, you no longer care about the processes in the container and are comfortable with killing them.
- `docker pause`: Send SIGSTOP(pause signal)
	- use when you only want to suspend the processes in the container; you do not want them to lose data or state.

## Actions
- get IP of container - `docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <my-container-name>`
