# Resource Requirements

* Each node has a set of CPU and Memory available
* Whenever a pot is placed on a node it consumes some of the resources available for that node
* The k8s-scheduler decides what node a pod goes to
* The scheduler takes into consideration the requirement of a pod and the available resources of that node
* If there are no resources the pod will be in pending state with an event describing the reason (usually Insufficient cpu)
* you can specify the amount of CPU and memory required for a pod

## Resource Requests

* Defines the minimum resources for a pod

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-web-app-color
    labels:
        name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
      
      resources:
        requests:
            memory: "4Gi"
            cpu: 2

```

## Resource - CPU

* The lowest value is `0.1` or `1m`, where `m` stands for "milli"
* 1 CPU = 1 vCPU (1 AWS vCPU, 1 GCP Core, 1 Azure Core, 1 Hyperthread)

## Resource - Memory

* G = gigabyte
* Gi = Gibibyte

## Resource limits

* By default a pod the resources are not limmited
* Both CPU and memory can be limitted
* Limits are set for each container within the pod

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: simple-web-app-color
    labels:
        name: simple-webapp-color
spec:
    containers:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
      
      resources:
        requests:
            memory: "4Gi"
            cpu: 2
        limits:
            memory: "2Gi"
            cpu: 2

```

## Exceed Limits

* When the CPU limits are hit - the CPU gets throttle
* A container can consume more memory than its limits, but if that happens constantly - it will be terminated with a OOM

## Default behavior

### Default CPU behavior

* Pods can consume however many resources they want
* Pods don't have any requests for resources

* When no requests are set but the limits are set, k8s sets the Requests = Limits

* When Requests and Limits are set each pod get the guaranteed number of CPU, but they can exceed the limits if the node has enough resources

* When Requests are set but no Limits - this is the scenario that makes sense most of the time, as a pod may consume however much CPU it need at a give time

### Default Memory behavior

* When no limits and no requests - a pod may consume all the available resources of a pod
* When no requests, but there are limits - the pod will fail with an OOM
* When Requests and Limits are set - each pod gets the requested resources
* When Requests are set but Limits are not set - pod will be terminated with an OOM

## LimitRange

* Allows defining default resource requests and limits to pod that do not have them set in the pod definition file
* The limits are applied when a pod is created
* Creating/updating a limit range will not affect existing pods

```yaml
apiVersion: v1
kind: LimitRange
metadata:
    name: cpu-resources-constraint
spec:
    limits:
    - default:
        cpu: 500m
      defaultRequest:
        cpu: 5000m
      max:
        cpu: "1"
      min:
        cpu: "100m"
      type: Container
```

```yaml
apiVersion: v1
kind: LimitRange
metadata:
    name: memory-resources-constraint
spec:
    limits:
    - default:
        memory: 1Gi
      defaultRequest:
        memory: 5000m
      max:
        memory: "1Gi"
      min:
        memory: "500Mi"
      type: Container
```

## Request Quotas

* Allows setting hand limits to Requests and Limits
* They are applied at the namespace level

```yaml
apiVersion: v1
kind: Resource
metadata:
    name: my-resource-quota
spec:
    hard:
        requests.cpu: 4
        requests.memroy: 4Gi
        limits.cpu: 10
        limits.memory: 10Gi
```