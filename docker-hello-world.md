# Docker Hello World

HelloWorld Cheatsheet for 
* installing Docker
* creating a small small c-program 
* creating a Docker Image containing and executing that c file with Dockerfile



##  Installing

```sh
# 2 options:
# this provided by the Linux distribution
sudo apt install docker.io
# this is provided by docker.inc
sudo apt-get install docker-ce docker-ce-cli containerd.io
#

```

## Install everything required for compiling basic software written in C and C++.
`sudo apt install build-essential`

## Create "hello.c"
```c
#include <stdio.h>

int main() {
    printf("Hello World!\n");
    return 0;
}
```

## Compile C-File
`gcc -o hello --static hello.c`

## Create Dockerfile
* create a file called "Dockerfile" with following content:

```yaml
# indicates that this image will start from an empty image
FROM scratch

# copies a file hello to the root filesystem
COPY hello /

# executes the hello program
CMD ["/hello"]
```

## Create Docker Image
`sudo docker image build -t helloimage .`

## Check Images and Containers
```
sudo docker images
sudo docker ps
```


## Start Docker Container
```sh
docker run helloimage

```









