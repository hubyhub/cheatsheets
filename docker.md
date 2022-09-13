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
docker images               # shows you all images, you have locally. 
docker logs containerId     # shows you the logs of the container. (you get the containerId by via docker ps)
docker run --name cool-name app:1.0   # gives your container a nice name, so you can reference it easier without containerId
docker exec -it containerId /bin/bash # gives you the terminal within the container, for debugging, etc...
docker network ls                     # shows docker networks, which is used by the containers to talk to each other
docker network create manfred-network # creates a new docker network

# full example
docker run -d -p6001:6379 --name my-cool-name app:1.0 
```

### exec -it
* gives you the terminal of a running container 
* it is a bit "limited" linux terminal (e.g.: no curl)
* -it stands for **interactive terminal
* you are logged in as root user
* you can navigate the virtual file system of the container
* print environment-variables etc..
* type "exit" if you want to exit interactive terminal

### Review docker run vs docker start
**docker run**: will take an image with a specific version, or the latest. So you are working with images. and it will create a new container
docker start on the other side, you are working with a container. It allows you to restart a stopped container. 


### Docker Network
Docker creates its own isolated network, where the containers are running in.
* So when we deploy 2 containers to the same docker network (e.g. mongodb & mongo Express Ui), they can talk to each other just with their container name. without localhost, portnumber etc...
* and applications that run outside of docker, like node.js needs to connect to them via localhost and portnumber
* so when we create a container out of node.js app, it will run in docker network and can talk to the other containers via container name
* by default docker provides you with auto-generated networks: **host, none docker_default, bridge ** `docker network ls`

## Workflow with Docker
How is Docker used in practice? In Software development you usually have following steps:
* Development
* Continuous Integration/ Delivery
* Deployment

**The question is, how does Docker integrate in all those steps?**
Scenario: 

* You are developing a Javascript Applicaiton, which uses MongoDB.
* And instead of installing it locally on your laptop, you download a docker container from Docker Hub.
* You connect your Javascript Application with MongoDB and you start developing.
* You commit your changes to git, so that another one can test the application.
* That will trigger a CI (a jenkins-build,...) 
  1. which will build the application and create artifacts from the application.
  2. and then will create a Docker Image from that Artifact
  3. the created image is then pushed to a (private) Docker Repository
  4. A DevServer pulls both images. official Mongoimage from DockerHub & AppImage from private Docker Repo
  5. The Containers can talk to each other and are configured and can be tested

### Steps:
* go to docker hub and search and downloadfor mongo-db and mongo express
  ```bash
  docker pull mongo
  docker pull mongo-express
  docker images
  ```
* run containers and setup connections via docker-network
  ```bash
  docker network create manfred-network
  ```
* have a look at documentation at docker hub how we can configure mongo container
```bash
  # -p open default-ports for host and container ports
  # -d run it in detached mode
  # -e set environment variables for mongo (from documentation)
  # --name sets the container-name. we need it to connect to mongo-express
  # --net is the network
  docker run -p 27017:27017 -d -e MONGO_INITDB_ROOT_USERNAME=admin -e MONGO_INITDB_ROOT_PASSWORD=password --name mongodb --net mongo-network mongo
  ```
* log container and check running container
  ```
  # check for "waiting for connections on port 27017"
  docker logs containerId
  ```





