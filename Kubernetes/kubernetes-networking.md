# K8S networking 101

* Every node gets an IP address
* Each pod gets its own IP address
* All pods connected to the same network can communicate with each other
* IPs are changed on pods are recreated

## Cluster Networking

K8S has some netwoking requirements:

* All containers/pods can communicate to one anotehr without NAT
* All nodes can communicate with all containers and vice versa without NAT
