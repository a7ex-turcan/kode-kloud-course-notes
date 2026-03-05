# Docker Run

## tag
* used to version images
* no tag = latest

## stdin
* by default a docker container doesn't listen to stdin
* to map the stdin of the host to the stdin of the container use `-i`
* to attach to the container's terminal use `-it`
 
## port mapping
* use `-p` param: `docker run -p <host-port:container-port>

## volume mapping
* use `-v` to map a host directory to a container directory:

```bash
docker run -v <host-dir>:<container-dir>
```

## inspect

* provides additional details for the container:
`docker inspect <docker-container>`

## logs

* `docker logs <container-id>`

## Linking containers together

* Allows containers to see eachother
`docker run --link redis:redis`

* under the hood this will create an entry in the `/etc/hosts` file
* Links are deprecated in favour of more modern ways
