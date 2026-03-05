# Docker compose

* configuration file in YML
* configures services
* allows spinning up complex services in a declarative fashion
* easier to maintain

## Example dockercompose

```dockercompose
version: 2

services:
    redis:
        image: redis
        networks:
            - back-end
    db:
        image: postgres:9.4
        networks:
            - back-end
    vote:
        # allows building the image from a dockerfile
        build: ./vote
        image: voting-app
        ports:
            - 5000:80
        depends_on:
            - redis
        networks:
            - front-end
            - back-end

    result:
        build: ./result
        image: result-app
        ports:
            - 5001:80
        links:
            - db
        networks:
            - front-end
            - back-end
    worker:
        build: ./worker
        image: worker
        links:
            - redis
            - db
        networks:
            - back-end

networks:
    front-end:

    backe-end:
```

## Running dockercompse

* To run the entire stack: `docker-compose up`

## Docker compose - versions

* Docker compose evolved over time
* For version 2 and up, the version should be stated explicitly
* version 2 creates a dedicated bridged network that connects all the containers in the docker compose file.
* link are not neccessary in v2 because of this
* v2 introduces `dependsOn`

* v3 supports docker swarm

## Networks in docker compose

