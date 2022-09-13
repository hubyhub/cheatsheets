# Docker
[Full Tutorial](https://www.youtube.com/watch?v=3c-iBn73dDE)
1) search docker-images on https://hub.docker.com/
2) download by using "pull" or "run" command

```bash
docker run postgres:9.6   # pulls image and starts container (postgres image with version 9.6) if version is omitted, it downloads the latest) 
```

A Pull downloads actually multiple image-layers (if version changes of one layer, only that layer needs to be downloaded by your local docker


## Commands
```docker pull imageName				# pulls image from repo to local environment
docker run        # combines docker pull & docker start (pulls image first, if it is not locally available)
docker start      # re-start stopped container
docker stop       # stops container
docker run -d     # detached mode, if console closes container still runs
docker run -d -p6000:6794 	# p defines portnumber of host-machine and port of container (hostmachine-port can be used only once)
docker ps                   # shows you which containers are currently running (and on which port)
docker ps -a                # shows you all containers, also the stopped ones
docker images               # gives you all images you have locally 
docker logs containerId     # gives you the logs of the container				
docker run --name cool-name app:1.0   # --name gives your container a nice name, so you can reference it easier 
```

### exec
with exec you can get the terminal of a running container (interactive terminal)
* you are root user
* you can navigate the virtual file system of the container
* print environment-variables etc..
* type "exit" to exit interactive terminal and return 
* it is a bit limited linux terminal (e.g.: no curl)
#### Example
```bash
docker exec -it containerId /bin/bash  
```
#### full example 
```
docker run -d -p6001:6379 --name my-cool-name app:1.0
```
