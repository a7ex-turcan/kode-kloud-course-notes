# Docker Engine and Storage

## The docker engine

The docker engine has 3 major parts:

1. The docker CLI
    * it can be on an entirely other host and connect remotely: `docker -H=remote-docker-engine:2375 <any command>`
2. The REST API
3. The Docker Deamon

### Restricting resourecs

Docker uses `cgroups` to restrict how much of a resource can a container use

```bash
# no more than 50% of the host's CPU
# no more than 100m of the host's memory
docker run --cpus=.5 --memory=100m ubuntu
```

## The docker storage

### File system

* docker data is at: `/var/lib/docker`

### Volumes

* allow persisting data across container restarts
* volumes are at: /var/lib/docker/volumes/<volume_name>
* to create a volume:

```bash
docker volume create data_volume
```

* to attach to a container

```bash
# If volume does not exist, it will be automatically created
docker run -v data_volume:/var/lib/mqsql mysql
```

* you can also attach a random path from the host as a volume:

```bash
docker run -v /data/mysql:/var/lib/mysql mysql
```

#### Mount type

1. Volume mount - mounts a volume
2. Bind mount - mounts a dir from any location on the host

#### Storage drivers:

* maintain the layered architecture

Examples include:

* AUFS
* ZFS
* BTRFS
* Device Mapper
* Overlay
* Overlay2