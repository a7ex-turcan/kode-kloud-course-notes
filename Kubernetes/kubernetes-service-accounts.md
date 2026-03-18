# Service Accounts

## User Account

* used by humans
* an admin access, a dev access etc.

## Service Account

* usde by machines
* an account used by an application to interact with the k8s cluster

* an identity of a service
* is associated with a token - used to authenticate the service

* the service account `default` is created in all namespaces

* to list the service accounts

```bash
kubectl get serivceaccount
```

```bash
kubectl describe serviceaccount default
```

* When a pod is created the `default` service account is automatically attached to the pod
* The Service Account is mounted as a `projected volume` within the pod at the location `/var/run/secrets/kubernetes.io/serviceaccount`
* The token of the service account should be available in the file `token`
* The token is unique for each pod
* The token automatically expires when the pod is terminated

### Creating a service account

```bash
kubectl create serviceaccount <service-account-name>
```

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
    name: dashboard-sa
    namespace: default

# used to prevent the automatic creation of the token
automountServiceAccountToken: false
```

### Associating a Service Account to a container

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: my-kubernetes-dashboard
spec:
    containers:
        - name: <container-name>
          image: <image>
    serviceAccountName: dashboard-sa
    
    # used to prevent the automatic creation of the token. takes precendece over the one set at the ServiceAccountLevel
    automuntServiceAccountToken: false
```

### Create Token

* When you want to want to use the token outside the cluster (e.g. a cUrl request etc.)
* All tokens are valid for 1h by default

```bash
kubectl create token dashboard-sa --duration 2h
```

* The token can then be used as a Bearer token

## Taints and Tolerations

* Used to set restrictions on what pods can be scheduled on a node
* Taints are placed over Nodes
* By default Pods have no tolerations
* Tolerations are placed on the Pods

### Taints

```bash
kubectl taint nodes node-name key=value:taint-effect
```

#### Taint effects

* `NoSchedule` - no pods will be scheduled on the node
* `PreferNoSchedule` - a pods may still be scheduled on that noe
* `NoExecute` - no new pods will be scheduled on the node and any existing pod will be evicted

## Tolerations

To add a tolleration on a pod

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: my-app
    spec:
        containers:
        - name: nginx-container
          image: nginx
        tolerations:
        # all these values need to be encoded in double quotes
        - key: "app"
          operator: "Equal"
          value: "blue"
          effect: "NoSchedule"
```
