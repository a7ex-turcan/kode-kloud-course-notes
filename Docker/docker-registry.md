# Docker Registry

* A centralized place that stores images

## Image name convetion

```bash
# library - the user or accout
# nginx - the name of the image/repository
# docker.io - the registry from where the image is pulled. if omitted docker.io will be used by default

image: docker.io/library/nginx`
```

### Other popular registries

* gcr.io - Google's registry

## Private Registries

* Provided by most cloud providers

* to login into a private registry: `docker login <registry>`

### Deploying a Private Registry

* The docker registry can be deployed on prem
* The image name: `registry`

* To Push:
    * tag the image with the registry name

    ```bash
    docker image tag my-image <registry>/myimage
    ```

    * Push:

    ```bash
    docker push <registry>/myimage
    ```
