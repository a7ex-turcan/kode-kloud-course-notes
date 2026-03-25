# Multi containers pods

* The containers share the same lifetime
* The network is also shared, they can refer to eachother as localhost
* Access to the same storage

## Create

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
    labels:
        name: simple-webapp
spec:
    containers:
    - name: web-app
      image: web-app
      ports:
      - containerPort: 8080
    - name: main-app
      image: main-app
```

## Design patterns

### Colocated containers

* Two containers running in a pod

### Regular Init Container

* Used when initialization of the main container is required
* The init container ends before the main container is started

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
    labels:
        name: simple-webapp
spec:
    containers:
    - name: web-app
      image: web-app
      ports:
      - containerPort: 8080
    initContainers:
    # init containers are run sequencialy
    - name: db-checker
      image: busybox

    - name: api-checker
      image: busybox
```

### Sidecar containers

* The side car containers starts first
* After the side car is ready the main container starts
* The side car stays running

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-webapp
    labels:
        name: simple-webapp
spec:
    containers:
    - name: web-app
      image: web-app
      ports:
      - containerPort: 8080
    initContainers:
    - name: log-shipper
      image: busybox
      command: 'setup-log-shipper.sh'
      # this is the important point
      restartPolicy: Always
   
```