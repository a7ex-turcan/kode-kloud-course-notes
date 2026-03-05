# Docker images

## How to containarize:

* you identify the steps required to deploy the app
* create a docker file containing the instructions to set up the app

```dockerfile
FROM <base-image>

RUN <instruction>

COPY <from> <to>

ENTRYPOINT <the entry point instruction>
```

* `docker build dockerfile -t <tag> .`
* the `.` is the context the docker build will run in
* `docker push <the tag from previous step>`

## The `dockerfile`

* specific format
* on the left: instructions
* on the right: argumets to the instruction

* `FROM` - the base image, either an OS, or another image that itself is based on an OS
* `RUN` - runs a particular command on the base image
* `COPY` - copies files from the local system onto the docker image
* `ENTRYPOINT` - the command that is run when the image is run as a container

* each line creates a layer
* to see each layer of a image:

```bash
docker history <imagename>
```

* layers can be cached

## Env variables

* to pass an env var to `docker run` use `-e VAR=VALUE`
* to find env vars of a container that's already running: `docker inpect`

## CMD VS ENTRYPOINT

### CMD

* You can have the command and args as json array: `["command", "param"]`
* A command is replaced when an explicit command is being passed with `docker run`

### ENTRYPOINT

* Allows appending arguments to the command when you do `docker run <image>`

* You can combine CMD and Entry point to define default args for the command defined in the `ENTRYPOINT`

* You can still overwrite the `ENTRYPOINT` by using the `--entyrpoint` flag
