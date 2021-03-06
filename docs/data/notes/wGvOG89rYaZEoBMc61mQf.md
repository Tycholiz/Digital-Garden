
## Watchtower
- allows us to update an already running docker container by simply pushing to Docker Hub. Watchtower sees that a new image has been pushed, automatically runs `docker pull`, then starts the container back up again.
- note: a message from calibre:
	- We do not endorse the use of Watchtower as a solution to automated updates of existing Docker containers. In fact we generally discourage automated updates. However, this is a useful tool for one-time manual updates of containers where you have forgotten the original parameters. In the long term, we highly recommend using Docker Compose.

## Portainer
an open source, platform agnostic tool for managing containerized applications. It works with Kubernetes, Docker, Docker Swarm, Azure ACI in both data centres and at the edge.
```
docker container run -d \
  -p 9000:9000 \
  -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer
```
[Website](https://www.portainer.io/)
