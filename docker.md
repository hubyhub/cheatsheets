# Docker
[Full Tutorial](https://www.youtube.com/watch?v=3c-iBn73dDE)
1) search docker-images on https://hub.docker.com/
2) download by using "pull" or "run" command

```bash
docker run postgres:9.6   # pulls image and starts container (postgres image with version 9.6) if version is omitted, it downloads the latest) 
```

A Pull downloads actually multiple image-layers (if version changes of one layer, only that layer needs to be downloaded by your local docker


## Commands
```
docker pull imageName       # pulls image from repo to local environment
docker run                  # combines docker pull & docker start (pulls image first, if it is not locally available)
docker start                # re-start stopped container
docker stop                 # stops container
docker run -d               # detached mode, if console closes container still runs
docker run -d -p6000:6794   # p defines portnumber of host-machine and port of container (hostmachine-port can be used only once)
docker ps                   # shows you which containers are currently running (and on which port)
docker ps -a                # shows you all containers, also the stopped ones
docker images               # shows you all images, you have locally. 
docker logs containerId     # shows you the logs of the container. (you get the containerId by via docker ps)
docker run --name cool-name app:1.0   # gives your container a nice name, so you can reference it easier without containerId
docker exec -it containerId /bin/bash # gives you the terminal within the container, for debugging, checking env-variables
docker exec -it containerId /bin/sh   # some containers don't have bash installed
env                                   # check environment variables, usefull to test if docker-file sets them correctly
docker network ls                     # shows docker networks, which is used by the containers to talk to each other
docker network create manfred-network # creates a new docker network

docker rm containerId       # deletes a container
docker rmi imageId          # deletes an image, you first have to delete the image, even when it is stopped

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
* go to docker hub and search and download for mongo-db and mongo express
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
* start mongo-express and connect it to running mongo-db on startup
  ```bash
  docker run
  -p 8081:8081                                  # host & container port
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin      # username for mongodb        
  -e ME_CONFIG_MONGODB_ADMINPASSOWRD=password   # password for mongodb
  -e ME_CONFIG_MONGODB_SERVER=mongodb           # the CONTAINER-NAME of mongodb-container
  --net mongo-network                           # docker-network name
  --name mongo-express                          # gives the container a name 
  mongo-express
  ```
* again log the container and lets have a look whats running in it:
  ```bash
  # should seay something like: 
  # "mongo express server llstening on htt://0.0.0.0:8081"
  # which can be opened in the browser localhost:8081
  docker logs containerId
  docker logs containerId | tail # display just last part of logs
  docker logs containerId -f     # stream the logs periodically
  ```
* create new mongodb- database via GUI in browser on localhost:8081 
  * by specifying a environment variable **init_DB** we could create a init database when starting the container
* Connect Node Server with MongoDB Container
  * check mongodb and mongo-express running `docker ps`
  * at PORTS you can see where mongo-db is listening in your code change it accordingly so something like:
     ```bash
     # username and password would not be hardcoded
     MongoClient.connect('mongodb://admin:password@localhost:27017');
     var db = client.db('user-account');
     ```
## Docker Compose (mongo-docker-compose.yaml)
* automates running docker containers
* takes your run commands from the terminal and converts its in a structured document and save it as .yaml file
* **image:**  We need to know from which image the container will be built from, you can also add a tag, if you want to have a specific version
* **network-configuration** is not in docker-compose (--net mongo-network). Docker Compose takes care of creating a common Network!
* **Indentation in yaml-Files is important!**
* if 2 containers depend on each other you could define a "waiting logic"
* when you restart a container, everything that you stored there, is gone. it is not persistent
* of course you want to persist data even when container stops, for that you need --> DOCKER VOLUMES

```
| docker run command                          | mongo-docker-compose.yaml                     |
|---------------------------------------------|-----------------------------------------------|
|                                             | version: '3'     # version of docker-compose  |   
|    docker run -d\                           | services:        # list of containers go here |   
|     --name mongodb\                         |    mongodb:      # container name             |   
|     -p 27017:27017\                         |      image:  mongo    # image name            |   
|     -e MONGO-INITDB_ROOT_USERNAME=admin\    |      ports:                                   |
|     -e MONGO-INITDB_ROOT_PASSWORD=password\ |       -27017:27017                            |
|     --net mongo-network\                    |      environment:                             |
|     mongo                                   |       -MONGO_INITDB_ROOT_USERNAME=admin       |
|                                             |       -MONGO_INITDB_ROOT_PASSWORD=password    |
|                                             |    mongo-express:                             |
|                                             |       image: mongo-express                    |
|                                             |       ports:                                  |
|                                             |        -8080:8080                             |
|                                             |       environment:                            |
|                                             |        -ME_CONFIG_MONGODB_ADMINUSERNAME=admin |
|                                             |        -ME_CONFIG_MONGODB_ADMINPASSWORD=pwd   |
|                                             |        -ME_CONFIG_MONGODB_SERVER=mongodb      |
```

Now you can start containers using docker-compose file

```bash
# -f which file should run
# up: starts all containers
# down: stops all containers and removes all networks
docker-compose -f mongo.yaml up
docker-compose -f mongo.yaml down
```

## Create your own Docker image with Dockerfile
* we wnat to package our javascript-App into our own Docker-Container
* Jenkins needs to build javascript Application first and then it will create the docker image from the content (artifact)

## Dockerfile = Blueprint for building images
* first line of every Dockerfile is `FROM imageName`
* whatever image you are creating, you always want to base it on another image
* in our case we need **node.js** image for our container
* instead of lower-level alpine image we can use a ready to use node-image from docker-hub
* whenever you adjust a docker-file you have to rebuld the image
* **FROM** 
* **ENV** - you can also define environment variables here. is an alternative to docker-compose
* **RUN** - execute any Linux command, this is executed INSIDE the container!!
* **COPY** - executes on the HOST machine!!
* **CMD** - is an **entrypoint command**, you can have only one CMD, but multiple RUN commands
```
FROM node                 # users node-image
ENV MONGO_DB_USERNAME=admin
RUN mkdir -p/home/app    # this folder will be created in the container and not on the host/notbook
COPY . /home/app         # copy current folder files from HOST-MACHINE TO INSIDE THE IMAGE /home/app
CMD["node", "/home/app/server.js"] # start the app with "node /home/app/server.js"
```


### How to build an image from a Docker file
We have to use `docker build` and provide 2 parameters:
* a name & tag: name your app as you which and provide a tag. can also be anything usually version
* location of docker-file. in our case it is in same directory. therefore "."

```bash
docker build -t my-app:1.0 .
```

Then the image is built, and an ID is created.
**Workflow in jenkins:** 
 * we have to commit the docker-file with our code and jenkins will create the image
 * we have to share the created image in a docker repository

## Create a private repository for Docker Images (=Docker Registry)
options: 
* AWS -> Amazon ECR (=Elastic Container Registry) there you have 1 repository per image, but u store different tags(versions) of the same image
* Nexus
* Digital Ocean

```
# usually you have to login with docker
docker login
# but aws gives you a command line tool do login and push to aws
$(aws ecr get-login....)
```

## Docker Volumes for persisting Data
* a directory of the host machine is mounted in the container
* 3 Types of Docker Volumes
  * Host volumes
    * `-v hostFolder:containerFolder` 
    * `docker run -v /home/mount/data:/var/lib/mysql/data`
    * you decide which folder of the **host file system ** you want to mount into the container
  * Anonymous Volumes
    * you just reference the container-directory without the host folder   
    * `-v /containerFolder`       
    * `docker run `-v /var/lib/mysql/data`
    * docker will create the folder automatically
  * Named Volumes (recommended & mostly used)
    * you specify the 'name' of the volume
    * you don't have to know the path to a host-folder
    * `docker run -v name:/var/lib/mysql/data`    
  * in docker-compose you define volumes
```
version: '3'
services: 
  mongodb: 
    image: mongo
    volumes: 
      -db-data:/var7lib/mysql/data
```
