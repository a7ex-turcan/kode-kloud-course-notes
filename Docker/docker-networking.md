# Docker networking

3 networks are installed automatically together with docker: 

* Bridge - the default
* none - not attached to any network
* host - no network isolation between the host and the container

To attach a container to a specific network

```bash
docker run Ubuntu --network=hst
```

## User defined networks

```bash
docker network create --driver bridge --subnet 182.18.0.0/16 custom-isolated-network
```

To list network: `docker network ls`

### Embeded DNS

* Containers can reach eachother by name
* This is take care by the built in docker DNS
* The built in DNS server runs at: `127.0.0.11`
