# Observability

## Pod Status

* Pending - the pod is pending for scheduling on a pod
* ContainerCreating - the image required for the container are being pulled
* Running - all the containers have been created and are running

## POD Conditions

All pod conditions, these conditions move from false to true, sequencially:

* PodScheduled
* Initialized
* ContainersReady
* Ready - is running and ready to accept user traffic

## The Ready status

* By default - k8s assumes that as soon as a pod is created it is ready to accept user status
* This may not always be true

## The Readiness Probe

* Allows to check if the application is actually ready
* For example: an http call to a probe endpoint, a TCP test to a port, run a script inside the container

```yaml
apiVersion: v1
kind: Pod
metadata: simple-webapp
    name: simple-webapp
    lables:
        name: simple-webapp
spec:
    containers:
    - name: simple-webapp
      image: simple-webapp
      ports:
      - containerPort: 8080
      readinessProbe:
        # makes an http request
        httpGet:
            path: /api/ready
            port: 80

        # for tcp
        tcpSocket:
            port: 3306
        
        # for a exec command
        exec:
            command:
            - cat
            - /app/is_ready

        # allows configuring an initial delay before excuting the probe
        initialDelay: 10

        # how often to probe
        periodSeconds: 5

        # overrides the default (3) attemts count
        failureThreshold: 8
```

## The Liveness Probes

* Can help to determine if the pod can actually process trafic
* If the liveness probe fails the pod will be terminated or restarted


```yaml
apiVersion: v1
kind: Pod
metadata: simple-webapp
    name: simple-webapp
    lables:
        name: simple-webapp
spec:
    containers:
    - name: simple-webapp
      image: simple-webapp
      ports:
      - containerPort: 8080
      livenessProbe:
        # makes an http request
        httpGet:
            path: /api/ready
            port: 80

        # for tcp
        tcpSocket:
            port: 3306
        
        # for a exec command
        exec:
            command:
            - cat
            - /app/is_ready

        # allows configuring an initial delay before excuting the probe
        initialDelaySeconds: 10

        # how often to probe
        periodSeconds: 5

        # overrides the default (3) attemts count
        failureThreshold: 8
```

## Logs

```bash
# Container name is only required for multicontainer pods
kubectl logs -f <pod-name> <container-name>
```

## Monitoring

