# Kubernetes Concepts

## Setting up kubernetes

### Local setup

* Use one of the following: docker desktop, minkube, kubeadm

### In cloud

* GCP: GKE
* AWS: EKS
* Azure: AKS

### Playgrounds

* kodekloud.com/k8s

## Pods

* Encapsulates containers
* A single instance of an application
* The smallest thing you can creat in k8s
* To scale an application you create more pods of the same app
* Pods can be deployed on any node of the cluster
* usually a 1:1 relationship to an application
* a pod can have multiple containers, but usually not of the same kind

* To run a pod:

```bash
kubectl run nginx --image nginx
```

* docker hub servers the image

* To get all pods

```bash
kubectl get pods
```

* To Edit a pod

```bash
kubectl edit pod 
```

### Pods with YAML

* K8s uses yml files as input to create various type of objects

* the structure is the same for all type objects

```yaml
# the version of the k8s api that will be used to create the objects
apiVersion: v1 or apps/v1

# the type of object we are creating
kind: Pod

# data about the object (name, labels etc)
metadata:
    name: myapp-pod
    labels:
        app: myapp
        type: front-end

# Info about the object we are creating.
# The format is different for different type of objects
spec:
    containers:
        - name: nginx-container
          image: nginx
          # Allows passing arguments into the container. same as vaing a CMD in the docker file
          args: ["10"]
          # Allows overriding the docker file's default ENTRYPOINT
          command: ["sleep"]
    
```

* To create the resource defined by the file:

```bash
kubectl create -f pod-definition.yml
```

* To list the pods: `kubectl get pods`
* To see detaile info about the pod: `kubectl describe pod myapp-pod`

* To extract a pod definition file from a pod

```bash
kubectl get pod -o yaml > pod-definition.yaml
```

## Replication Controllers and ReplicaSets

### The Replication Controller

* Ensures that the specified number of pods are running at all times
* Ensures load balancing and scaling
* The Replication Controller is the older technology

#### Creating a Replication Controller

```yaml
apiVersion: v1

kind: ReplicationController

metadata:
    name: myapp-rc
    labels:
        app: myapp
        type: front-end

spec:
    # the template of a pd
    template:
        # data about the object (name, labels etc)
        metadata:
            name: myapp-pod
            labels:
                app: myapp
                type: front-end

        # Info about the object we are creating.
        # The format is different for different type of objects
        spec:
            containers:
                - name: nginx-container
                  image: nginx
    
    replicas: 3
```

* The command to create it: `kubectl create -f rc-definition.yml`
* To view the replication controllers: `kubectl get replicationcontroller`

### The ReplicaSet

* The new recommended way
* Still the same purpose

#### Creating a replica set

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
    name: myapp-replicaset
    labels:
        app: myapp
        type: front-end

spec:
    template:
        <pod-definition>
    replicas: <number of pods>
    selector:
        matchLabels:
            type: front-end
```

* The command to create it: `kubectl create -f rs-definition.yml`
* To view the replication controllers: `kubectl get replicaset`

### Labels and Selectors

* Labels help group pods together
* The selectors help replicasets (and other resources) identify pods by their labels

### Scaling a replica set

```bash
kubectl scale --replicas=6 -f replicaset-definition.yml
```

or

```bash
kubectl scale --replicas=6 replicaset myapp-replicaset
```

## Deployments

* One layer above ReplicaSet
* Provides rolling updates
* Undo changes

* Deployment definition file:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
    name: myapp-deployment
    labels:
        app: myapp
        type: front-end

spec:
    template:
        metadata:
            name: myapp-pod
            labels:
                app: myapp
                type: front-end
            spec:
                containers:
                - name: nginx-container
                  image: nginx
    
    replicas: 3

    selector:
        matchLabels:
            type: front-end

```

* To get deployments: `kubectl get deployments`
* To get all created resources: `kubectl get all`

### Update and Rollback

#### Rollout and Versioning

* with each new rollout a `revision` is created
* enables rolling back to a specific revision

* to see the satus of a rollout

```bash
kubectl rollout status deployment/<deployment-name>
```

* to see the history:

```bash
kubectl rollout history deployment/<deployment-name>
```

#### Deployment strategies

1. The `Recreate` strategy - old containers get droped - new containers get created
2. The `Rolling` strategy - the default strategy

#### Rollbacks

```bash
kubectl rollout undo deployment myapp-deployment
```

## Namespaces

* the default namespace: `default`
* the internal services of k8s are in the: `kube-system`
* `kube-public`: resources that are availalbe to all users
* namespaces allow for isolation
* each namespace can have its own set of policies
* each namespace can also have a defined quota of resources

* to create a namespacce

* a service in one namespace can reach a service in another namespace by appending the namespace name to the name of the service:

```bash
# cluster.loca - the domain of the cluster
# svc - the subdomain for services
# dev - the namespace
# db-service - the name of the service
ping db-service.dev.svc.cluser.local
```

* to list pods in a namespace

```bash
kubeclt get pods --namespace=kube-sys
```

* to create a pod in another namespace

```bash
kubectl create -f <definition-file> --namespace dev
```

* namespace can also be defined in the YAML Definition file

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: myapp-pod
    namespace: dev

....
```

* to create a namespace

```yaml
apiVersion: v1
kind: Namespace
metadata:
    name: dev
```

* to switch to a given namespace

```bash
kubectl config set-context $(kubectl config current-context) --namespace=dev
```

* to view all pods from all namespaces

```bash
kubectl get pods --all-namespaces
```

### Resource quota

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
    name: compute-quota
    namespace: dev

spec:
    hard:
        pods: "10"
        requests.cpu: "4"
        requests.memroy: 5Gi
        limits.cpu: "10"
        limits.memory: 10Gi
```

## Env Vars

* To set an env variable when creating a pod via a definition file:

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: sample-app
spec:
    containers:
        - name: sample-app-container
          image: image-name
          env:
            - name: MyEnvVar
              value: MyEnvVarValue
```

## Config Maps

* Makes it easy to share env vars between different pods
* Centralizes configuration data

* To view the config maps:

```bash
kubectl get configmaps
```

* To Configure config maps:
  * Create the config map
    * Imperative:

        ```bash
        kubectl create configmap <config-name> \
        --from-literal=key=value \
        --from-literal=key1=value1
        ```

        or

        ```bash
        kubectl create configmap <config-name> \
        --from-file=app_config.properties
        ```

    * Declarative

    ```yaml definition.yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
        name: app-config

    data:
        APP_COLOR: blue
        APP_MODE: prod
    ```

    then

    ```bash
    kubectl create -f definition.yaml
    ```

  * Inject them in the pod

    ```yaml
    apiVersions: v1
    kind: Pod
    metadata:
        name: simple-webapp-colo
    spec:
        containers:
        - name: my-container
          image: my-image
          
          envFrom:
          # Links the pod to the entire config map
          - configMapRef:
            name: app-config
          
          env:
            - name: APP_COLOR
              # binds the APP_COLOR env var to a value defined in the app-config config map
              valueFrom:
                configMapKeyRef:
                    name: app-config
                    key: APP_COLOR
          
          volumes:
          - name: app-config-volume
            configMap:
                name: app-config
    ```

## Secrets

* Used to store passwords and sensitive info

* To View secrets:

```bash
kubectl get secretes
```

* To view details together with the values

```bash
kubectl get secret <secret-name> -o yaml
```

* The `imperative` way of creating a secret

```bash
kubectl create secret generic <secret-name> \
--from-literal=<key>=<value>
```

or

```bash
kubectl create secret generic <secret-name> \
--from-file=app_secret.properties
```

* The `declarative` way of creating secrets

```yaml secret.yaml
apiVersion: v1
kind: Secret

metadata:
    name: app-secret

data:
    DB_Host: encoded
    DB_User: encoded
    DB_Passwowrd: encoded
```

then

```bash
kubectl create -f secret-data.yaml
```

### Encoding secrets

On linux:

```bash
echo -n 'mysql' | base64
```

### Decoding secrets

On linux:

```bash
echo -n <the encoded value> | base64 --decode
```

### Injecting Secrets into a pod

```yaml
apiVersions: v1
kind: Pod
metadata:
    name: simple-webapp-colo
spec:
    containers:
    - name: my-container
    image: my-image
    
    envFrom:
    # Links the pod to the entire secret file
    - secretRef:
            name: app-secret
    
    env:
        - name: APP_COLOR
        # binds the APP_COLOR env var to a value defined in the app-config config map
        valueFrom:
            secretKeyRef:
                name: app-secret
                key: APP_COLOR
    
    # Mounts the entire secret file as one file per secret
    # The name of teh file is the secret key, and the contents: the value
    volumes:
    - name: app-secret-volume
        configMap:
            name: app-secret
```
