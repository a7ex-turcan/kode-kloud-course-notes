# Docker commands

## Basic commands

* `run` - runs a container

```bash
docker run nginx
```

* to run in detached mode: `docker run -d <image>`

* `docker ps` list all running containers
    * `-a` - shows all containers 

* `docker stop <container name|container id>` stops a container
* `docker rm <container>` - remove a container 
* `docker images` - list available images 
* `docker rmi <image>` - removes an image
* `docker pull <image>` - pulls an image without image
* `docker exec <container> <command>` executes a command inside the container 
    * `-i` flag allows you to intercat with the docker container

* `docker attach <container>` - attaches to a container
