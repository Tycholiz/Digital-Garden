
- `docker logs <container name>` - see what happened during container initialization
- `docker inspect <container-name>` - get low-level info about a container, such as its IP, port mappings,  
- remove all containers based on one image (ex. monica) - `docker ps -a | awk '{ print $1,$2 }' | grep monica | awk '{print $1 }' | xargs -I {} docker rm {}`
