
##### Remove all containers built from Image 7401323eb08e
```
docker ps -a | awk '{ print $1,$2 }' | grep 7401323eb08e | awk '{print $1 }' | xargs -I {} docker rm {}
```
