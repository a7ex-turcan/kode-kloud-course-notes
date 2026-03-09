# K8S Services

Services enable communcation between services within and outside the application
Help connect apps togheter

## Service Ports

* NodePort: Listens to a port on a node and forwards requests to the pod
* ClusterIP: service creates a virtual IP inside the cluster
* LoadBalancer: provides a load balancing

### NodePort

Allows for external access by mapping a node port ot a pod port

TargetPort: the port on the pod
The Port: the port on the service itself
The NodePort: the port on the node (30000-32767)

#### The definition file

```yaml
apiVersion: v1
kind: Service
metadata:
    name: myapp-service

spec:
    type: NodePort
    ports:
          # if not provided, the `port` will be used
        - targetPort: 80
          # mandatory
          port: 80
          # is assigned a random value if not assigned
          nodePort: 30008
    
    selector:
        # the labes as defined in the pod-definition.yml
        app: myapp
        type: front-end
```

* If the `selector` matches multiple pods - the service will cover all matched pods, and balance the load beween them using the `Random` algorithm

* To list services

```bash
kubectl get services
```

## ClusterIP

* helps group pods together
* Is the default type of a service in k8s
* The pods can be accessed via the service name or the service IP

### The ClusterIP definition file

```bash
apiVersion: v1
kind: Service

metadata:
    name: back-end

spec:
    type: ClusterIP
    ports:
        - targetPort: 80
          ports: 80
    
    selector:
        # the labes as defined in the pod-definition.yml
        app: myapp
        type: back-end
```

## Load Balancer

* Some cloud platforms provide native support for k8s' `LoadBalancer` service type
