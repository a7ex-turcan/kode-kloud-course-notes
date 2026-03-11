# Docker Security

## Process isolation

* Docker containers are not completely isolated from the host
* The container are still run on the host's kernel
* From the container's POV - the process id of the container process is 1
* However in the OS - the container process has a different process ID
* This is done via Namespaces

## User

* by default docker runs the processes inside the contianers using the `root` user
* the root user within the container - isn't really like the `root` user of the host
* Docker uses `Linux Capabilities` to implement this "container root" vs "host root" isolation
* The Capabilities live at `/usr/include/linux/capability.h`
* By default docker runs the contianer with a limitted set of capabilities hence the root in the docker container has fewer permissions than the host root user

* To add more capabilities:

```bash
docker run --cap-add MAC_ADMIN ubuntu
```

* To remove capabilities:

```bash
docker run --cap-drop KILL ubuntu
```

* To run the container with all privileges enabled:

```bash
docker run --privileged
```

* to run as another user

```bash
docker run --user=<user-id> <image>
```

or via the Dockerfile

```dockerfile
FROM ubuntu

USER <user-id>
```